---
title: "ASP.NET Core işleme hatası"
author: ardalis
description: "ASP.NET Core uygulamaları hataların nasıl işleneceğini bulur."
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49507e90cd659be5da08df17e175297adad0fea1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="20335-103">Hata ASP.NET çekirdek işleme giriş</span><span class="sxs-lookup"><span data-stu-id="20335-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="20335-104">Tarafından [Steve Smith](https://ardalis.com/) ve [zel Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="20335-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="20335-105">Bu makalede, ASP.NET Core uygulamaları hataları işlemek için ortak appoaches yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="20335-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="20335-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="20335-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="20335-107">Geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="20335-107">The developer exception page</span></span>

<span data-ttu-id="20335-108">Özel durumlar hakkında ayrıntılı bilgiler gösterilmektedir bir sayfasını görüntülemek için uygulamayı yapılandırmak için yükleme `Microsoft.AspNetCore.Diagnostics` NuGet paketini ve bir satırı ekleyin [başlangıç sınıfında yöntemini yapılandırma](startup.md):</span><span class="sxs-lookup"><span data-stu-id="20335-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="20335-109">PUT `UseDeveloperExceptionPage` Ara yazılımların, istediğiniz gibi özel durumları yakalamak için önce `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="20335-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="20335-110">Geliştirici özel durum sayfasını etkinleştir **yalnızca uygulama geliştirme ortamında çalışırken**.</span><span class="sxs-lookup"><span data-stu-id="20335-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="20335-111">Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık şekilde paylaşma istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="20335-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="20335-112">[Ortamlarını yapılandırma hakkında daha fazla bilgi](environments.md).</span><span class="sxs-lookup"><span data-stu-id="20335-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="20335-113">Geliştirici özel durum sayfasını görmek için ayarlanan ortam ile örnek uygulamayı çalıştırın `Development`ve ekleme `?throw=true` uygulamanın temel URL.</span><span class="sxs-lookup"><span data-stu-id="20335-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="20335-114">Sayfa özel durumu ve istek hakkında bilgi içeren birkaç sekme içerir.</span><span class="sxs-lookup"><span data-stu-id="20335-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="20335-115">İlk sekme yığın izlemesi içerir.</span><span class="sxs-lookup"><span data-stu-id="20335-115">The first tab includes a stack trace.</span></span> 

![Yığın izleme](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="20335-117">Sonraki sekme sorgu dizesi parametreleri varsa gösterir.</span><span class="sxs-lookup"><span data-stu-id="20335-117">The next tab shows the query string parameters, if any.</span></span>

![Sorgu dizesi parametreleri](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="20335-119">Bu istek tanımlama bilgilerine sahip oldu, ancak olsaydı, üzerinde görünecekleri **tanımlama bilgilerini** sekmesi. Son sekmede geçirilmiş üstbilgileri görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20335-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Üstbilgileri](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="20335-121">Sayfa işleme özel bir durum yapılandırma</span><span class="sxs-lookup"><span data-stu-id="20335-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="20335-122">Uygulama çalışmadığı zaman kullanmak için bir özel durum işleyici sayfasını yapılandırmak için iyi bir fikirdir `Development` ortamı.</span><span class="sxs-lookup"><span data-stu-id="20335-122">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="20335-123">Bir MVC uygulamasında açıkça HTTP yöntemi öznitelikleriyle hata işleyici eylem yöntemi gibi tasarlamanız yok `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="20335-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="20335-124">Açık fiillerini kullanarak bazı istekleri yöntemi ulaşmasını engellemeniz.</span><span class="sxs-lookup"><span data-stu-id="20335-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="20335-125">Yapılandırma durumu kod sayfaları</span><span class="sxs-lookup"><span data-stu-id="20335-125">Configuring status code pages</span></span>

<span data-ttu-id="20335-126">Varsayılan olarak, uygulamanızı zengin durum kod sayfası için HTTP durum kodları 500 (Dahili Sunucu hatası) veya 404 (bulunamadı) gibi sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="20335-126">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="20335-127">Yapılandırabileceğiniz `StatusCodePagesMiddleware` bir satıra ekleyerek `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="20335-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="20335-128">Varsayılan olarak, bu ara yazılımın 404 gibi ortak durum kodları için basit, yalnızca metin işleyiciler ekler:</span><span class="sxs-lookup"><span data-stu-id="20335-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 sayfası](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="20335-130">Ara yazılım birkaç farklı genişletme yöntemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="20335-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="20335-131">Lambda ifadesi alır, başka bir içerik türü ve biçimi dizesini alır.</span><span class="sxs-lookup"><span data-stu-id="20335-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="20335-132">Yeniden yönlendirme uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="20335-132">There are also redirect extension methods.</span></span> <span data-ttu-id="20335-133">Bir 302 durum kodunu istemciye gönderir ve bir istemciye özgün durum kodunu döndüren ancak işleyicinin yeniden yönlendirme URL'si de yürütür.</span><span class="sxs-lookup"><span data-stu-id="20335-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="20335-134">Durum kod sayfaları belirli istekleri için devre dışı bırakmanız gerekirse, bunu yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="20335-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="20335-135">Özel durum işleme kodu</span><span class="sxs-lookup"><span data-stu-id="20335-135">Exception-handling code</span></span>

<span data-ttu-id="20335-136">Özel durum işleme sayfaları kodda istisnalar atabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20335-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="20335-137">Genellikle, yalnızca statik içeriği oluşması üretim hata sayfaları için iyi bir fikir olabilir.</span><span class="sxs-lookup"><span data-stu-id="20335-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="20335-138">Ayrıca, yanıt üstbilgileri gönderildikten sonra yanıtın durum kodu değişiklik yapamaz veya herhangi bir özel durum sayfaları veya işleyicileri çalıştırabilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="20335-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="20335-139">Yanıt doldurulmalıdır veya bağlantı durduruldu.</span><span class="sxs-lookup"><span data-stu-id="20335-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="20335-140">Sunucu özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="20335-140">Server exception handling</span></span>

<span data-ttu-id="20335-141">Özel durum işleme mantığı, uygulamanızda yanı sıra [server](servers/index.md) uygulamanızı barındırma bazı özel durum işleme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="20335-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="20335-142">Üstbilgileri gönderilmeden önce sunucunun bir özel durum yakalar, sunucunun hiçbir gövde ile 500 İç sunucu hatası yanıt gönderir.</span><span class="sxs-lookup"><span data-stu-id="20335-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="20335-143">Üstbilgileri gönderildikten sonra sunucu bir özel durum yakalar, sunucu bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="20335-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="20335-144">Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="20335-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="20335-145">Sunucunun bir özel durum nedeniyle oluşan herhangi bir özel durum işlenmiş işleme.</span><span class="sxs-lookup"><span data-stu-id="20335-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="20335-146">Herhangi bir özel hata sayfaları yapılandırılabilir veya özel durum işleme ara yazılımı veya filtreler bu davranışını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="20335-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="20335-147">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="20335-147">Startup exception handling</span></span>

<span data-ttu-id="20335-148">Yalnızca barındırma katmanı uygulama başlatma sırasında gerçekleşmesi özel durumlar işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="20335-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="20335-149">Yapabilecekleriniz [nasıl konak hatalarına yanıt olarak başlatma sırasında davranacağını yapılandırmak](hosting.md#detailed-errors) kullanarak `captureStartupErrors` ve `detailedErrors` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="20335-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="20335-150">Ana bilgisayar adresini/bağlantı noktası sonra bağlama hatası oluşursa Barındırma yakalanan başlatma hatası için bir hata sayfası yalnızca gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="20335-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="20335-151">Hiçbir bağlama herhangi bir nedenle başarısız olursa, barındırma katman kritik bir özel durumla, dotnet işlem çökmesi günlüğe kaydeder ve hiçbir hata sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="20335-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="20335-152">ASP.NET MVC hata işleme</span><span class="sxs-lookup"><span data-stu-id="20335-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="20335-153">[MVC](../mvc/index.md) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="20335-153">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="20335-154">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="20335-154">Exception Filters</span></span>

<span data-ttu-id="20335-155">Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="20335-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="20335-156">Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durum işleme ve aksi durumda adlı değil.</span><span class="sxs-lookup"><span data-stu-id="20335-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="20335-157">Özel durum filtreleri hakkında daha fazla bilgi [filtreleri](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="20335-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="20335-158">Özel durum filtreleri içinde MVC Eylemler oluşan özel durumlarını yakalama için iyi, ancak bunlar hata ara yazılım işleme kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="20335-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="20335-159">Ara yazılımı genel örneği için tercih ettiğiniz ve hata işleme yapmak için yalnızca ihtiyaç duyacağınız filtreleri kullanın *farklı* MVC eylemi seçildi tabanlı.</span><span class="sxs-lookup"><span data-stu-id="20335-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="20335-160">İşleme Model durumu hataları</span><span class="sxs-lookup"><span data-stu-id="20335-160">Handling Model State Errors</span></span>

<span data-ttu-id="20335-161">[Model doğrulama](../mvc/models/validation.md) çağrılan her denetleyici eylemi öncesinde gerçekleşir ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki.</span><span class="sxs-lookup"><span data-stu-id="20335-161">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="20335-162">Model doğrulama hataları, ilgilenmek için standart bir kural, bu durumda izlemeniz gereken bazı uygulamalar seçecektir bir [filtre](../mvc/controllers/filters.md) böyle bir ilke uygulamak için uygun bir yerdir olabilir.</span><span class="sxs-lookup"><span data-stu-id="20335-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="20335-163">Geçersiz model durumlarıyla eylemlerinizi nasıl davranacağını test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="20335-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="20335-164">Daha fazla bilgi edinin [denetleyicisi mantığının test edilmesi](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="20335-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



