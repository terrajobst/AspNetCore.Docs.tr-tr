---
title: "ASP.NET Core işleme hatası"
author: ardalis
description: "ASP.NET Core uygulamaları hataların nasıl işleneceğini bulur."
keywords: "ASP.NET Core, hata işleme, özel durum işleme"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: de2ba0ff9ad17c198c06b510ecfb49f808721bdf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="f4e28-104">Hata ASP.NET çekirdek işleme giriş</span><span class="sxs-lookup"><span data-stu-id="f4e28-104">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="f4e28-105">Tarafından [Steve Smith](https://ardalis.com/) ve [zel Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="f4e28-105">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="f4e28-106">Bu makalede, ASP.NET Core uygulamaları hataları işlemek için ortak appoaches yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="f4e28-106">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="f4e28-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f4e28-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="f4e28-108">Geliştirici özel durumu sayfası</span><span class="sxs-lookup"><span data-stu-id="f4e28-108">The developer exception page</span></span>

<span data-ttu-id="f4e28-109">Özel durumlar hakkında ayrıntılı bilgiler gösterilmektedir bir sayfasını görüntülemek için uygulamayı yapılandırmak için yükleme `Microsoft.AspNetCore.Diagnostics` NuGet paketini ve bir satırı ekleyin [başlangıç sınıfında yöntemini yapılandırma](startup.md):</span><span class="sxs-lookup"><span data-stu-id="f4e28-109">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="f4e28-110">PUT `UseDeveloperExceptionPage` Ara yazılımların, istediğiniz gibi özel durumları yakalamak için önce `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="f4e28-110">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="f4e28-111">Geliştirici özel durum sayfasını etkinleştir **yalnızca uygulama geliştirme ortamında çalışırken**.</span><span class="sxs-lookup"><span data-stu-id="f4e28-111">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="f4e28-112">Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık şekilde paylaşma istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4e28-112">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="f4e28-113">[Ortamlarını yapılandırma hakkında daha fazla bilgi](environments.md).</span><span class="sxs-lookup"><span data-stu-id="f4e28-113">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="f4e28-114">Geliştirici özel durum sayfasını görmek için ayarlanan ortam ile örnek uygulamayı çalıştırın `Development`ve ekleme `?throw=true` uygulamanın temel URL.</span><span class="sxs-lookup"><span data-stu-id="f4e28-114">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="f4e28-115">Sayfa özel durumu ve istek hakkında bilgi içeren birkaç sekme içerir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-115">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="f4e28-116">İlk sekme yığın izlemesi içerir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-116">The first tab includes a stack trace.</span></span> 

![Yığın izleme](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="f4e28-118">Sonraki sekme sorgu dizesi parametreleri varsa gösterir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-118">The next tab shows the query string parameters, if any.</span></span>

![Sorgu dizesi parametreleri](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="f4e28-120">Bu istek tanımlama bilgilerine sahip oldu, ancak olsaydı, üzerinde görünecekleri **tanımlama bilgilerini** sekmesi. Son sekmede geçirilmiş üstbilgileri görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4e28-120">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Üstbilgileri](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="f4e28-122">Sayfa işleme özel bir durum yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f4e28-122">Configuring a custom exception handling page</span></span>

<span data-ttu-id="f4e28-123">Uygulama çalışmadığı zaman kullanmak için bir özel durum işleyici sayfasını yapılandırmak için iyi bir fikirdir `Development` ortamı.</span><span class="sxs-lookup"><span data-stu-id="f4e28-123">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="f4e28-124">Bir MVC uygulamasında açıkça HTTP yöntemi öznitelikleriyle hata işleyici eylem yöntemi gibi tasarlamanız yok `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="f4e28-124">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="f4e28-125">Açık fiillerini kullanarak bazı istekleri yöntemi ulaşmasını engellemeniz.</span><span class="sxs-lookup"><span data-stu-id="f4e28-125">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="f4e28-126">Yapılandırma durumu kod sayfaları</span><span class="sxs-lookup"><span data-stu-id="f4e28-126">Configuring status code pages</span></span>

<span data-ttu-id="f4e28-127">Varsayılan olarak, uygulamanızı zengin durum kod sayfası için HTTP durum kodları 500 (Dahili Sunucu hatası) veya 404 (bulunamadı) gibi sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="f4e28-127">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="f4e28-128">Yapılandırabileceğiniz `StatusCodePagesMiddleware` bir satıra ekleyerek `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f4e28-128">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="f4e28-129">Varsayılan olarak, bu ara yazılımın 404 gibi ortak durum kodları için basit, yalnızca metin işleyiciler ekler:</span><span class="sxs-lookup"><span data-stu-id="f4e28-129">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 sayfası](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="f4e28-131">Ara yazılım birkaç farklı genişletme yöntemleri destekler.</span><span class="sxs-lookup"><span data-stu-id="f4e28-131">The middleware supports several different extension methods.</span></span> <span data-ttu-id="f4e28-132">Lambda ifadesi alır, başka bir içerik türü ve biçimi dizesini alır.</span><span class="sxs-lookup"><span data-stu-id="f4e28-132">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="f4e28-133">Yeniden yönlendirme uzantı yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="f4e28-133">There are also redirect extension methods.</span></span> <span data-ttu-id="f4e28-134">Bir 302 durum kodunu istemciye gönderir ve bir istemciye özgün durum kodunu döndüren ancak işleyicinin yeniden yönlendirme URL'si de yürütür.</span><span class="sxs-lookup"><span data-stu-id="f4e28-134">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="f4e28-135">Durum kod sayfaları belirli istekleri için devre dışı bırakmanız gerekirse, bunu yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="f4e28-135">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="f4e28-136">Özel durum işleme kodu</span><span class="sxs-lookup"><span data-stu-id="f4e28-136">Exception-handling code</span></span>

<span data-ttu-id="f4e28-137">Özel durum işleme sayfaları kodda istisnalar atabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4e28-137">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="f4e28-138">Genellikle, yalnızca statik içeriği oluşması üretim hata sayfaları için iyi bir fikir olabilir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-138">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="f4e28-139">Ayrıca, yanıt üstbilgileri gönderildikten sonra yanıtın durum kodu değişiklik yapamaz veya herhangi bir özel durum sayfaları veya işleyicileri çalıştırabilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f4e28-139">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="f4e28-140">Yanıt doldurulmalıdır veya bağlantı durduruldu.</span><span class="sxs-lookup"><span data-stu-id="f4e28-140">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="f4e28-141">Sunucu özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="f4e28-141">Server exception handling</span></span>

<span data-ttu-id="f4e28-142">Özel durum işleme mantığı, uygulamanızda yanı sıra [server](servers/index.md) uygulamanızı barındırma bazı özel durum işleme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-142">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="f4e28-143">Üstbilgileri gönderilmeden önce sunucunun bir özel durum yakalar, sunucunun hiçbir gövde ile 500 İç sunucu hatası yanıt gönderir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-143">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="f4e28-144">Üstbilgileri gönderildikten sonra sunucu bir özel durum yakalar, sunucu bağlantıyı kapatır.</span><span class="sxs-lookup"><span data-stu-id="f4e28-144">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="f4e28-145">Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-145">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="f4e28-146">Sunucunun bir özel durum nedeniyle oluşan herhangi bir özel durum işlenmiş işleme.</span><span class="sxs-lookup"><span data-stu-id="f4e28-146">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="f4e28-147">Herhangi bir özel hata sayfaları yapılandırılabilir veya özel durum işleme ara yazılımı veya filtreler bu davranışını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="f4e28-147">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="f4e28-148">Başlangıç özel durum işleme</span><span class="sxs-lookup"><span data-stu-id="f4e28-148">Startup exception handling</span></span>

<span data-ttu-id="f4e28-149">Yalnızca barındırma katmanı uygulama başlatma sırasında gerçekleşmesi özel durumlar işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-149">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="f4e28-150">Yapabilecekleriniz [nasıl konak hatalarına yanıt olarak başlatma sırasında davranacağını yapılandırmak](hosting.md#detailed-errors) kullanarak `captureStartupErrors` ve `detailedErrors` anahtarı.</span><span class="sxs-lookup"><span data-stu-id="f4e28-150">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="f4e28-151">Ana bilgisayar adresini/bağlantı noktası sonra bağlama hatası oluşursa Barındırma yakalanan başlatma hatası için bir hata sayfası yalnızca gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-151">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="f4e28-152">Hiçbir bağlama herhangi bir nedenle başarısız olursa, barındırma katman kritik bir özel durumla, dotnet işlem çökmesi günlüğe kaydeder ve hiçbir hata sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-152">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="f4e28-153">ASP.NET MVC hata işleme</span><span class="sxs-lookup"><span data-stu-id="f4e28-153">ASP.NET MVC error handling</span></span>

<span data-ttu-id="f4e28-154">[MVC](../mvc/index.md) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="f4e28-154">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="f4e28-155">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="f4e28-155">Exception Filters</span></span>

<span data-ttu-id="f4e28-156">Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-156">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="f4e28-157">Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durum işleme ve aksi durumda adlı değil.</span><span class="sxs-lookup"><span data-stu-id="f4e28-157">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="f4e28-158">Özel durum filtreleri hakkında daha fazla bilgi [filtreleri](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="f4e28-158">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="f4e28-159">Özel durum filtreleri içinde MVC Eylemler oluşan özel durumlarını yakalama için iyi, ancak bunlar hata ara yazılım işleme kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-159">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="f4e28-160">Ara yazılımı genel örneği için tercih ettiğiniz ve hata işleme yapmak için yalnızca ihtiyaç duyacağınız filtreleri kullanın *farklı* MVC eylemi seçildi tabanlı.</span><span class="sxs-lookup"><span data-stu-id="f4e28-160">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="f4e28-161">İşleme Model durumu hataları</span><span class="sxs-lookup"><span data-stu-id="f4e28-161">Handling Model State Errors</span></span>

<span data-ttu-id="f4e28-162">[Model doğrulama](../mvc/models/validation.md) çağrılan her denetleyici eylemi öncesinde gerçekleşir ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki.</span><span class="sxs-lookup"><span data-stu-id="f4e28-162">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="f4e28-163">Model doğrulama hataları, ilgilenmek için standart bir kural, bu durumda izlemeniz gereken bazı uygulamalar seçecektir bir [filtre](../mvc/controllers/filters.md) böyle bir ilke uygulamak için uygun bir yerdir olabilir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-163">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="f4e28-164">Geçersiz model durumlarıyla eylemlerinizi nasıl davranacağını test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f4e28-164">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="f4e28-165">Daha fazla bilgi edinin [denetleyicisi mantığının test edilmesi](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="f4e28-165">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



