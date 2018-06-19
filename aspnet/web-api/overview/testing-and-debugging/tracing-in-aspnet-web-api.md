---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: ASP.NET Web API 2 izleme | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API izlemenin nasıl etkinleştirileceği gösterir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7392ae5d9bc4c3aab45a9373099a0ee18e873a4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044214"
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="6d8c0-103">ASP.NET Web API 2 izleme</span><span class="sxs-lookup"><span data-stu-id="6d8c0-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="6d8c0-104">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6d8c0-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="6d8c0-105">Web tabanlı bir uygulama hata ayıklama çalışırken izleme günlükleri iyi bir dizi için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="6d8c0-106">Bu öğreticide, ASP.NET Web API izlemenin nasıl etkinleştirileceği gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="6d8c0-107">Web API çerçevesi önce ve sonra denetleyiciyi çağıran yaptıklarını izlemek için bu özelliği kullanın.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="6d8c0-108">Kendi kodunuzu izlemek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6d8c0-109">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="6d8c0-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6d8c0-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (Visual Studio 2015 ile de çalışır)</span><span class="sxs-lookup"><span data-stu-id="6d8c0-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="6d8c0-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="6d8c0-111">Web API 2</span></span>
> - [<span data-ttu-id="6d8c0-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="6d8c0-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="6d8c0-113">Web API izleme System.Diagnostics etkinleştir</span><span class="sxs-lookup"><span data-stu-id="6d8c0-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="6d8c0-114">İlk olarak, yeni bir ASP.NET Web uygulaması projesi oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="6d8c0-115">Visual Studio'da gelen **dosya** menüsünde, select **yeni**, ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="6d8c0-116">Altında **şablonları**, **Web**seçin **ASP.NET Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="6d8c0-117">Web API projesi şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="6d8c0-118">Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**, ardından **paket yönetmek konsol**.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="6d8c0-119">Paket Yöneticisi konsolu penceresinde aşağıdaki komutları yazın.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="6d8c0-120">İlk komut, en son Web API izleme paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="6d8c0-121">Ayrıca, çekirdek Web API paketleri güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="6d8c0-122">İkinci komut WebApi.WebHost paketi en son sürüme güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="6d8c0-123">Belirli bir Web API sürümünü hedeflemek istediğiniz kullanırsanız izleme paketi yüklediğinizde sürüm bayrağı.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="6d8c0-124">Uygulamada WebApiConfig.cs dosyasını açın\_başlangıç klasörü.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="6d8c0-125">Aşağıdaki kodu ekleyin **kaydetmek** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="6d8c0-126">Bu kod ekler [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) Web API ardışık düzene sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="6d8c0-127">**SystemDiagnosticsTraceWriter** sınıfı Yazar izlemeleri için [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="6d8c0-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="6d8c0-128">İzlemelerini görmek için hata ayıklayıcısı'ndaki uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="6d8c0-129">Tarayıcıda gidin `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="6d8c0-130">İzleme deyimleri için Visual Studio çıktı penceresinde yazılır.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="6d8c0-131">(Gelen **Görünüm** menüsünde, select **çıkış**).</span><span class="sxs-lookup"><span data-stu-id="6d8c0-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="6d8c0-132">Çünkü **SystemDiagnosticsTraceWriter** izlemeler için Yazar **System.Diagnostics.Trace**, ek izleme dinleyicileri kaydedebilirsiniz; Örneğin, yazmak için izler bir günlük dosyasına.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="6d8c0-133">İzleme yazıcılarının hakkında daha fazla bilgi için bkz: [izleme dinleyicileri](https://msdn.microsoft.com/library/4y5y10s7.aspx) MSDN'de konu.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="6d8c0-134">Configuring SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="6d8c0-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="6d8c0-135">Aşağıdaki kod, izleme yazıcısı yapılandırma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="6d8c0-136">Kontrol edebilirsiniz iki ayarı vardır:</span><span class="sxs-lookup"><span data-stu-id="6d8c0-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="6d8c0-137">IsVerbose: false ise, en düşük miktarda bilgiyi her izlemeyi içerir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="6d8c0-138">TRUE ise, izlemeleri daha fazla bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-138">If true, traces include more information.</span></span>
- <span data-ttu-id="6d8c0-139">MinimumLevel: minimum izleme düzeyini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="6d8c0-140">Hata ayıklama, bilgi, uyar, hata ve önemli sırada izleme düzeyleri verilebilir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="6d8c0-141">Web API uygulamanıza izlemeleri ekleme</span><span class="sxs-lookup"><span data-stu-id="6d8c0-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="6d8c0-142">İzleme yazıcısı ekleme Web API ardışık düzen tarafından oluşturulan izlemeleri anında erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="6d8c0-143">Kendi kodunuzu izlemek için izleme yazıcısı de kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6d8c0-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="6d8c0-144">İzleme yazıcısı almak için arama **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="6d8c0-145">Bu yöntem bir denetleyicisinden üzerinden erişilebilir **ApiController.Configuration** özelliği.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="6d8c0-146">Bir izleme yazmak için çağırabilirsiniz **ITraceWriter.Trace** yöntemi doğrudan, ancak [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) sınıfı daha kolay bazı genişletme yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="6d8c0-147">Örneğin, **bilgisi** yukarıda gösterilen yöntemi ile izleme düzeyi izleme oluşturur **bilgisi**.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="6d8c0-148">Web API izleme altyapısı</span><span class="sxs-lookup"><span data-stu-id="6d8c0-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="6d8c0-149">Bu bölüm, özel izleme yazıcısı Web API'si yazma açıklar.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="6d8c0-150">Microsoft.AspNet.WebApi.Tracing paketi Web API'sinde daha genel izlemenin altyapısının en üstünde yerleşik olarak bulunur.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="6d8c0-151">Microsoft.AspNet.WebApi.Tracing kullanmak yerine, ayrıca bazı diğer izleme/oturum Kitaplığı'nda, aşağıdaki gibi ekleyebilirsiniz [NLog](http://nlog-project.org/) veya [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="6d8c0-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="6d8c0-152">İzlemeleri toplamak için uygulama **ITraceWriter** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="6d8c0-153">Basit bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="6d8c0-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="6d8c0-154">**ITraceWriter.Trace** yöntemi, bir izleme oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="6d8c0-155">Çağıran bir kategori ve izleme düzeyini belirtir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="6d8c0-156">Kategori herhangi bir kullanıcı tarafından tanımlanan dize olabilir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-156">The category can be any user-defined string.</span></span> <span data-ttu-id="6d8c0-157">Uygulamanıza **izleme** aşağıdakileri yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="6d8c0-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="6d8c0-158">Yeni bir **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="6d8c0-159">Bu istek, kategori ve izleme düzeyi ile gösterildiği gibi başlatın.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="6d8c0-160">Bu değerler, çağıran tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="6d8c0-161">Çağırma *traceAction* temsilci.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="6d8c0-162">Bu temsilci içinde çağıran içinde kalan doldurması beklenir **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="6d8c0-163">Yazma **TraceRecord**, istediğiniz herhangi bir günlük teknik kullanma.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="6d8c0-164">Burada gösterilen örnek içine basitçe çağırır **System.Diagnostics.Trace**.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="6d8c0-165">İzleme yazıcısı ayarlama</span><span class="sxs-lookup"><span data-stu-id="6d8c0-165">Setting the Trace Writer</span></span>

<span data-ttu-id="6d8c0-166">İzlemeyi etkinleştirmek için Web API'ı kullanacak şekilde yapılandırmanız gerekir, **ITraceWriter** uygulaması.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="6d8c0-167">Bunu aracılığıyla **HttpConfiguration** aşağıdaki kodda gösterildiği gibi nesnesi:</span><span class="sxs-lookup"><span data-stu-id="6d8c0-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="6d8c0-168">Yalnızca bir izleme yazıcısı etkin olabilir.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-168">Only one trace writer can be active.</span></span> <span data-ttu-id="6d8c0-169">Varsayılan olarak, Web API ayarlar bir &quot;yok&quot; hiçbir şey yapmaz İzleyici.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="6d8c0-170">( &quot;Yok&quot; İzleyici var. böylece izleme yazıcısı olup olmadığını denetlemek izleme kodu yok **null** izleme yazmadan önce.)</span><span class="sxs-lookup"><span data-stu-id="6d8c0-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="6d8c0-171">Nasıl Web API'si Works izleme</span><span class="sxs-lookup"><span data-stu-id="6d8c0-171">How Web API Tracing Works</span></span>

<span data-ttu-id="6d8c0-172">İçinde bir Web API'sini kullanan Web API'sini kullanır izleme bir *cephesi* Desen: izleme etkinleştirildiğinde izleme çağrıları gerçekleştirmek sınıflarıyla istek ardışık düzenini çeşitli kısımlarını Web API sarmalar.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="6d8c0-173">Örneğin, bir denetleyici seçerken, ardışık düzen kullanır **IHttpControllerSelector** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="6d8c0-174">İzleme etkin pipleline uygulayan bir sınıf ekler **IHttpControllerSelector** ancak gerçek uygulamaya çağrıları üzerinden:</span><span class="sxs-lookup"><span data-stu-id="6d8c0-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Web API izlemesi cephesi desen kullanır.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="6d8c0-176">Bu tasarımın avantajları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6d8c0-176">The benefits of this design include:</span></span>

- <span data-ttu-id="6d8c0-177">İzleme yazıcısı eklemezseniz izleme bileşenleri değil örneği ve performansını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="6d8c0-178">Varsayılan Hizmetleri gibi değiştirirseniz **IHttpControllerSelector** izleme sarmalayıcı nesne tarafından yapıldığından kendi özel bir uygulama ile izleme, etkilenmez.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="6d8c0-179">Varsayılan değiştirerek kendi özel framework ile tüm Web API izleme çerçevesini değiştirebilir **ITraceManager** hizmeti:</span><span class="sxs-lookup"><span data-stu-id="6d8c0-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="6d8c0-180">Uygulama **ITraceManager.Initialize** izleme sisteminizi başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="6d8c0-181">Bu değiştirir unutmayın *tüm* tüm Web API yerleşik izleme kodu da dahil olmak üzere, izleme framework.</span><span class="sxs-lookup"><span data-stu-id="6d8c0-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
