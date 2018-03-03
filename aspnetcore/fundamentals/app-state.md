---
title: ASP.NET Core oturum ve uygulama durumu
author: rick-anderson
description: "Koruma uygulama ve kullanıcı (oturum) durumunu yaklaşımları istekler arasında."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 6b81cadf39c0db373f82b8de7d8d3901d51ea088
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="9e9ec-103">ASP.NET Core oturum ve uygulama durumunda giriş</span><span class="sxs-lookup"><span data-stu-id="9e9ec-103">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="9e9ec-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), ve [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="9e9ec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="9e9ec-105">HTTP durum bilgisi olmayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="9e9ec-106">Bir web sunucusu her HTTP isteği bağımsız bir istek olarak değerlendirir ve kullanıcı değerlerini önceki isteklerinden korumaz.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-106">A web server treats each HTTP request as an independent request and doesn't retain user values from previous requests.</span></span> <span data-ttu-id="9e9ec-107">Bu makalede, uygulama ve istekler arasında oturum durumunu korumak için farklı yolları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-107">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="9e9ec-108">oturum durumu</span><span class="sxs-lookup"><span data-stu-id="9e9ec-108">Session state</span></span>

<span data-ttu-id="9e9ec-109">Oturum durumunu kaydetmek ve kullanıcı web uygulamanızı gözatar sırasında kullanıcı verilerini depolamak için kullanabileceğiniz ASP.NET Core bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-109">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="9e9ec-110">Sunucu üzerindeki bir sözlük veya karma tablo oluşan, oturum durumu verileri üzerinden yapılan istekler bir tarayıcıdan devam eder.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-110">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="9e9ec-111">Oturumunun veri önbelleği tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-111">The session data is backed by a cache.</span></span>

<span data-ttu-id="9e9ec-112">ASP.NET Core her istek ile sunucuya gönderilen oturum kimliği içeren bir tanımlama bilgisi istemci vererek oturum durumunu korur.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-112">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="9e9ec-113">Sunucu, oturum verilerini almak için oturum kimliği kullanır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-113">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="9e9ec-114">Oturum tanımlama bilgisi tarayıcıya özgü olduğundan, tarayıcılar arasında oturumları paylaşamaz.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-114">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="9e9ec-115">Yalnızca tarayıcı oturumu sona erdiğinde oturum tanımlama bilgileri silinir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-115">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="9e9ec-116">Süresi dolmuş bir oturum için bir tanımlama bilgisi alınmazsa, aynı oturum tanımlama bilgisi kullanan yeni bir oturum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-116">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="9e9ec-117">Sunucunun son istekten sonra sınırlı bir süre için bir oturum korur.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-117">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="9e9ec-118">Oturum zaman aşımını ayarlamanız veya 20 dakikalık varsayılan değeri kullanın.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-118">Either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="9e9ec-119">Oturum durumu, belirli bir oturuma özeldir, ancak kalıcı olarak kalıcı olmasını gerektirmez kullanıcı verilerini depolamak için idealdir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-119">Session state is ideal for storing user data that's specific to a particular session but doesn't need to be persisted permanently.</span></span> <span data-ttu-id="9e9ec-120">Veri silindiğinden yedekleme depolama alanından ya da çağrılırken `Session.Clear` veya oturum veri deposunda ne zaman sona erer.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-120">Data is deleted from the backing store either when calling `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="9e9ec-121">Sunucu, tarayıcı kapatıldığında veya oturum tanımlama bilgisi silindiğinde bilmiyor.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-121">The server doesn't know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="9e9ec-122">Hassas verileri oturumunda depolamayın.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-122">Don't store sensitive data in session.</span></span> <span data-ttu-id="9e9ec-123">İstemci değil Tarayıcıyı kapatın ve oturum tanımlama bilgisi temizleyin (ve bazı tarayıcılar oturum tanımlama bilgileri windows Canlı).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-123">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="9e9ec-124">Ayrıca, bir oturum için tek bir kullanıcı kısıtlı olmayabilir; sonraki kullanıcı ile aynı oturumu devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-124">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="9e9ec-125">Bellek içi oturum sağlayıcısı, yerel sunucuda oturum verilerini depolar.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-125">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="9e9ec-126">Bir sunucu grubunda web uygulamanızı çalıştırmak planlıyorsanız, belirli bir sunucuya her oturum bağlamanın Yapışkan oturumları kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-126">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="9e9ec-127">Windows Azure Web siteleri platform varsayılan Yapışkan oturumlarına (uygulama isteği yönlendirme veya ARR).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-127">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="9e9ec-128">Ancak, Yapışkan oturumları ölçeklenebilirliği etkileyen ve web uygulama güncelleştirmeleri zorlaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-128">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="9e9ec-129">Daha iyi bir seçenek Redis kullanmaktır veya SQL Server dağıtılmış, hangi Yapışkan oturumları gerektirmeyen önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-129">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="9e9ec-130">Daha fazla bilgi için bkz: [dağıtılmış bir önbellekle çalışmaya](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-130">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="9e9ec-131">Hizmet sağlayıcıları ayarlama hakkında daha fazla bilgi için bkz: [yapılandırma oturum](#configuring-session) bu makalenin ilerisinde yer.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-131">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="9e9ec-132">TempData</span><span class="sxs-lookup"><span data-stu-id="9e9ec-132">TempData</span></span>

<span data-ttu-id="9e9ec-133">ASP.NET Core MVC sunan [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliği bir [denetleyicisi](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-133">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="9e9ec-134">Bu özellik, dosyayı okuma kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-134">This property stores data until it's read.</span></span> <span data-ttu-id="9e9ec-135">`Keep` Ve `Peek` yöntemleri, verileri silme olmadan incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-135">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="9e9ec-136">`TempData` birden çok tek bir istek için veri gerektiğinde yeniden yönlendirme için özellikle yararlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-136">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="9e9ec-137">`TempData` TempData sağlayıcıları tarafından Örneğin, tanımlama bilgilerini veya oturum durumu kullanılarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-137">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="9e9ec-138">TempData sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="9e9ec-138">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e9ec-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e9ec-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9e9ec-140">ASP.NET Core 2.0 ve daha sonra tanımlama bilgisi tabanlı TempData sağlayıcısı TempData tanımlama bilgilerini depolamak için varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-140">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="9e9ec-141">Tanımlama bilgisi verileri ile kodlanmış [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-141">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="9e9ec-142">Tanımlama bilgisinin şifrelenir ve öbekli çünkü tek tanımlama bilgisi ASP.NET 1.x uygulanmaz çekirdek bulunan sınır boyutu.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-142">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="9e9ec-143">Şifrelenmiş verileri sıkıştırmak için güvenlik sorunları gibi yol açabileceğinden tanımlama bilgisi verileri sıkıştırılmış [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlali](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-143">The cookie data is not compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="9e9ec-144">Tanımlama bilgisi tabanlı TempData sağlayıcısı hakkında daha fazla bilgi için bkz: [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-144">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e9ec-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e9ec-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9e9ec-146">ASP.NET Core 1.0 ve 1.1, oturum durumu TempData sağlayıcısı varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-146">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="9e9ec-147">TempData sağlayıcısı seçme</span><span class="sxs-lookup"><span data-stu-id="9e9ec-147">Choosing a TempData provider</span></span>

<span data-ttu-id="9e9ec-148">TempData sağlayıcısı seçme bazı noktalar gibi içerir:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-148">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="9e9ec-149">Uygulama zaten başka bir amaçla oturum durumu kullanıyor mu?</span><span class="sxs-lookup"><span data-stu-id="9e9ec-149">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="9e9ec-150">Bu durumda, oturum durumu TempData sağlayıcısı kullanarak uygulama (yanı sıra veri boyutu) için ek ücret ödemeden sahiptir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-150">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="9e9ec-151">Uygulama TempData yalnızca tutumlu görece küçük miktarda veri (en fazla 500 bayt) kullanıyor mu?</span><span class="sxs-lookup"><span data-stu-id="9e9ec-151">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="9e9ec-152">Böylece, tanımlama bilgisi TempData sağlayıcı küçük bir maliyeti her istek ekleyeceğinizi, TempData taşır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-152">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="9e9ec-153">Aksi durumda, oturum durumu TempData sağlayıcısı TempData tüketilen kadar gidiş büyük miktarda veri her istekte önlemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-153">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="9e9ec-154">Uygulama bir web grubunda (birden çok sunucu) çalışıyor mu?</span><span class="sxs-lookup"><span data-stu-id="9e9ec-154">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="9e9ec-155">Bu durumda, tanımlama bilgisi TempData sağlayıcısı kullanmak için ek yapılandırma yoktur.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-155">If so, there's no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="9e9ec-156">Çoğu web istemcileri (örneğin, web tarayıcıları) her tanımlama bilgisi, tanımlama bilgilerinin toplam sayısı veya her ikisi de en büyük boyutu sınırları uygulayın.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-156">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="9e9ec-157">Bu nedenle, tanımlama bilgisi TempData sağlayıcı kullanırken, uygulama bu sınırı aşan olmaz doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-157">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="9e9ec-158">Şifreleme ek yüklerini için hesap oluşturma ve parçalama verilerin toplam boyutu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-158">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="9e9ec-159">TempData sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9e9ec-159">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e9ec-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e9ec-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9e9ec-161">Tanımlama bilgisi tabanlı TempData sağlayıcısı varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-161">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="9e9ec-162">Aşağıdaki `Startup` sınıf kodu oturum tabanlı TempData sağlayıcı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-162">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e9ec-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e9ec-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9e9ec-164">Aşağıdaki `Startup` sınıf kodu oturum tabanlı TempData sağlayıcı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-164">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="9e9ec-165">Sıralama ara yazılımı bileşenleri için kritik öneme sahiptir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-165">Ordering is critical for middleware components.</span></span> <span data-ttu-id="9e9ec-166">Önceki örnekte, türünde bir özel durum `InvalidOperationException` oluşur, `UseSession` sonra çağrılan `UseMvcWithDefaultRoute`.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-166">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="9e9ec-167">Bkz: [ara yazılım sıralama](xref:fundamentals/middleware/index#ordering) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-167">See [Middleware Ordering](xref:fundamentals/middleware/index#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9e9ec-168">.NET Framework hedefleme ve oturum tabanlı sağlayıcısını kullanarak eklerseniz, [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet paketini projenize.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-168">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="9e9ec-169">Sorgu dizeleri</span><span class="sxs-lookup"><span data-stu-id="9e9ec-169">Query strings</span></span>

<span data-ttu-id="9e9ec-170">Verileri sınırlı miktarda bir istekten başka bir yeni isteğin sorgu dizesi için ekleyerek geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-170">You can pass a limited amount of data from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="9e9ec-171">Bu durum e-posta veya sosyal ağlar paylaşılan katıştırılmış durumuyla bağlantılarına izin verir kalıcı bir şekilde yakalamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-171">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="9e9ec-172">Ancak, bu nedenle, hiçbir zaman sorgu dizeleri için hassas verileri kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-172">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="9e9ec-173">Kolayca paylaşılmasını ek olarak, sorgu dizelerine verileri dahil olmak üzere fırsatı oluşturabilirsiniz [siteler arası istek sahteciliği (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) kullanıcıların kimlik doğrulaması sırasında kötü amaçlı siteleri ziyaret içine kandırarak saldırıları.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-173">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="9e9ec-174">Saldırganlar uygulamanızdan kullanıcı verileri çalmaya veya kullanıcı adına kötü amaçlı eylemleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-174">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="9e9ec-175">Korunan application veya session durumu CSRF saldırılarına karşı korumanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-175">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="9e9ec-176">CSRF saldırıları hakkında daha fazla bilgi için bkz: [önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-176">For more information on CSRF attacks, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="9e9ec-177">Gönderme verisi ve Gizli alanlar</span><span class="sxs-lookup"><span data-stu-id="9e9ec-177">Post data and hidden fields</span></span>

<span data-ttu-id="9e9ec-178">Veri gizli form alanlarını kaydedilir ve bir sonraki istekte geri gönderilen.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-178">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="9e9ec-179">Bu, çok sayfalı formlarında yaygındır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-179">This is common in multi-page forms.</span></span> <span data-ttu-id="9e9ec-180">İstemci olası verileri değiştirme olduğundan, Bununla birlikte, sunucu her zaman bu düzeltin gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-180">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="9e9ec-181">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="9e9ec-181">Cookies</span></span>

<span data-ttu-id="9e9ec-182">Tanımlama bilgileri, web uygulamalarında kullanıcıya özgü verileri depolamak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-182">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="9e9ec-183">Tanımlama bilgilerini içeren tüm istekleri gönderildiğinden, kendi boyutu en az olarak tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-183">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="9e9ec-184">İdeal olarak, yalnızca bir tanımlayıcı bir tanımlama bilgisinde sunucuda depolanan gerçek verilerle depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-184">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="9e9ec-185">Çoğu tarayıcısı tanımlama bilgilerini 4096 bayt kısıtlayın.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-185">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="9e9ec-186">Ayrıca, tanımlama bilgileri, yalnızca sınırlı sayıda her etki alanı için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-186">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="9e9ec-187">Tanımlama bilgilerini oynama tabi olduğundan, bunlar sunucuda doğrulanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-187">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="9e9ec-188">Bir istemcide tanımlama bilgisinin dayanıklılık kullanıcı müdahalesi ve sona erme tarihi tabi olsa da, bunlar genellikle istemci üzerindeki veri kalıcılığını en sağlam biçiminde demektir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-188">Although the durability of the cookie on a client is subject to user intervention and expiration, they're generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="9e9ec-189">Tanımlama bilgileri, genellikle içeriği bilinen bir kullanıcı için burada özelleştirilmiş kişiselleştirme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-189">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="9e9ec-190">Kullanıcı yalnızca tanımlanır ve çoğu durumda kimlik doğrulaması değil çünkü, genellikle bir tanımlama bilgisi kullanıcı adı, hesap adı veya benzersiz bir kullanıcı kimliği (örneğin, bir GUID) tanımlama bilgisinde depolayarak güvenliğini sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-190">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="9e9ec-191">Tanımlama bilgisinin sonra bir sitenin kullanıcı kişiselleştirme altyapı erişmek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-191">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="9e9ec-192">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="9e9ec-192">HttpContext.Items</span></span>

<span data-ttu-id="9e9ec-193">`Items` Koleksiyonudur gerekli olan verileri depolamak için iyi bir konum belirli bir isteği işlerken yalnızca.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-193">The `Items` collection is a good location to store data that's needed only while processing one particular request.</span></span> <span data-ttu-id="9e9ec-194">Koleksiyonun içeriğini sonra her istek atılır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-194">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="9e9ec-195">`Items` Koleksiyonu en iyi bileşenler veya ara yazılım için bir yol olarak farklı noktalarda isteği sırasında zaman çalışır ve parametreleri geçirmek için hiçbir doğrudan şekilde iletişim kurmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-195">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="9e9ec-196">Daha fazla bilgi için bkz: [HttpContext.Items ile çalışma](#working-with-httpcontextitems), bu makalenin ilerisinde yer.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-196">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="9e9ec-197">Önbellek</span><span class="sxs-lookup"><span data-stu-id="9e9ec-197">Cache</span></span>

<span data-ttu-id="9e9ec-198">Önbelleğe alma, depolamak ve veri almak için etkili bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-198">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="9e9ec-199">Yaşam süresi ve diğer noktalar göre önbelleğe alınmış öğelerin kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-199">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="9e9ec-200">Daha fazla bilgi edinmek [önbelleğe alma](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-200">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="9e9ec-201">Oturum durumu ile çalışma</span><span class="sxs-lookup"><span data-stu-id="9e9ec-201">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="9e9ec-202">Oturum yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9e9ec-202">Configuring Session</span></span>

<span data-ttu-id="9e9ec-203">`Microsoft.AspNetCore.Session` Paket oturum durumu yönetmek için ara yazılım sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-203">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="9e9ec-204">Oturum Ara etkinleştirmek için `Startup` içermelidir:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-204">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="9e9ec-205">Herhangi bir [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) bellek önbellekleri.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-205">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="9e9ec-206">`IDistributedCache` Uygulama oturumu için bir yedekleme deposu olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-206">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="9e9ec-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) çağrısı, NuGet paketi "Microsoft.AspNetCore.Session" gerektirir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="9e9ec-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) çağırın.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="9e9ec-209">Aşağıdaki kod, bellek içi oturum Sağlayıcısı'nı ayarlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-209">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e9ec-210">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e9ec-210">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e9ec-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e9ec-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="9e9ec-212">Oturumdan başvurabilir `HttpContext` yüklenmiş ve yapılandırılmış sonra.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-212">You can reference Session from `HttpContext` once it's installed and configured.</span></span>

<span data-ttu-id="9e9ec-213">Erişmeye çalıştığınızda `Session` önce `UseSession` çağrılıp çağrılmadığını, özel durum `InvalidOperationException: Session has not been configured for this application or request` atılır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-213">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="9e9ec-214">Yeni bir oluşturmayı denerseniz `Session` (diğer bir deyişle, oturum tanımlama bilgisi oluşturulup oluşturulmadığını) yazmak zaten başlamıştır sonra `Response` akışı, özel durum `InvalidOperationException: The session cannot be established after the response has started` atılır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-214">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="9e9ec-215">Özel web sunucusu günlüğünde bulunabilir; tarayıcıda görüntülenmez.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-215">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="9e9ec-216">Oturum zaman uyumsuz olarak yükleme</span><span class="sxs-lookup"><span data-stu-id="9e9ec-216">Loading Session asynchronously</span></span> 

<span data-ttu-id="9e9ec-217">Arka plandaki gelen oturum kaydı ASP.NET Core varsayılan oturum sağlayıcısında yükler [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) depolama zaman uyumsuz olarak yalnızca IF [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) yöntemi önce açıkça çağrılır  `TryGetValue`, `Set`, veya `Remove` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-217">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="9e9ec-218">Varsa `LoadAsync` ilk olarak, temel olarak adlandırılmaz oturum kayıt yüklendiği zaman uyumlu olarak, hangi olası ölçeklendirmenizi uygulamanın etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-218">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="9e9ec-219">Uygulamanız bu deseni zorlamak için Kaydır [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) ve [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) uygulamaları, bir özel durum sürümleri ile `LoadAsync` yöntemi değil önce adlı `TryGetValue`, `Set`, veya `Remove`.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-219">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="9e9ec-220">Sarmalanan sürümleri hizmetler kapsayıcısının kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-220">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="9e9ec-221">Uygulama Ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="9e9ec-221">Implementation details</span></span>

<span data-ttu-id="9e9ec-222">Oturum tanımlama bilgisi izlemek ve tek bir tarayıcıdan istekleri belirlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="9e9ec-223">Varsayılan olarak, bu tanımlama bilgisi adı ". AspNet.Session"ve onu kullanan bir yolu"/".</span><span class="sxs-lookup"><span data-stu-id="9e9ec-223">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="9e9ec-224">Tanımlama bilgisi varsayılan bir etki alanı belirtin değil çünkü, istemci tarafı komut dosyası için sayfada kullanılamayacağı (çünkü `CookieHttpOnly` varsayılan olarak `true`).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-224">Because the cookie default doesn't specify a domain, it's not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="9e9ec-225">Oturum Varsayılanları geçersiz kılmak için kullanın `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-225">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9e9ec-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9e9ec-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9e9ec-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9e9ec-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="9e9ec-228">Sunucunun kullandığı `IdleTimeout` içeriğinin terk önce ne kadar oturum boşta kalabileceği belirlemek için özellik.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-228">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="9e9ec-229">Bu özellik tanımlama bilgisinin süre sonu bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-229">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="9e9ec-230">(Okuma veya yazma) oturum Ara geçtiği her istek zaman aşımını sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-230">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="9e9ec-231">Çünkü `Session` olan *kilitleme*, her ikisi de denemesi oturum, son içeriğini değiştirmek iki istek ilk kılıyorsa.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-231">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="9e9ec-232">`Session` olarak uygulanan bir *tutarlı oturum*, yani tüm içeriği birlikte depolanır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-232">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="9e9ec-233">(Farklı anahtarlar) oturum farklı bölümlerini değiştirmek iki isteği hala etkisi birbirine.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-233">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="9e9ec-234">Ayarlama ve oturum değerleri alma</span><span class="sxs-lookup"><span data-stu-id="9e9ec-234">Setting and getting Session values</span></span>

<span data-ttu-id="9e9ec-235">Oturumu aracılığıyla erişilir `Session` özelliği `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-235">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="9e9ec-236">Bu özellik bir [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-236">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="9e9ec-237">Aşağıdaki örnek, ayarlama ve int ve bir dize alma gösterir:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-237">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="9e9ec-238">Aşağıdaki genişletme yöntemleri eklerseniz, ayarlayın ve oturumuna serileştirilebilir nesneler alın:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-238">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="9e9ec-239">Aşağıdaki örnek, ayarlama ve alma serileştirilebilir bir nesne gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-239">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="9e9ec-240">HttpContext.Items ile çalışma</span><span class="sxs-lookup"><span data-stu-id="9e9ec-240">Working with HttpContext.Items</span></span>

<span data-ttu-id="9e9ec-241">`HttpContext` Özet türü bir sözlük koleksiyonu için destek sağlar `IDictionary<object, object>`adlı `Items`.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-241">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="9e9ec-242">Bu koleksiyon başından kullanılabilir bir *HttpRequest* ve her istek sonunda atılır.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-242">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="9e9ec-243">Anahtarlı bir girdi için bir değer atayarak ya da belirli bir anahtar değeri isteyen erişebilir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-243">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="9e9ec-244">Aşağıdaki örnekte, [ara yazılımı](xref:fundamentals/middleware/index) ekler `isVerified` için `Items` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-244">In the following sample, [Middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="9e9ec-245">Ardışık başka bir ara yazılım, erişebilir:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-245">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="9e9ec-246">Yalnızca tek bir uygulama tarafından kullanılacak olan ara yazılımı `string` anahtarları kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-246">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="9e9ec-247">Ancak, uygulamalar arasında paylaşılır Ara herhangi olasılığını anahtar çakışmaları önlemek için benzersiz nesne anahtarları kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-247">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="9e9ec-248">Birden çok uygulama arasında çalışmalıdır ara yazılımı geliştiriyorsanız, aşağıda gösterildiği gibi ara yazılımı sınıfında tanımlanan benzersiz nesne anahtarı kullanın:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-248">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="9e9ec-249">Başka bir kod depolanan değerine erişebilirsiniz `HttpContext.Items` ara yazılım sınıfı tarafından kullanıma sunulan anahtarı kullanarak:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-249">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="9e9ec-250">Bu yaklaşım, aynı zamanda "Sihirli dizelerde" kod birden fazla yerde yinelenmesinin ortadan avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-250">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="9e9ec-251">Uygulama durumu verileri</span><span class="sxs-lookup"><span data-stu-id="9e9ec-251">Application state data</span></span>

<span data-ttu-id="9e9ec-252">Kullanım [bağımlılık ekleme](xref:fundamentals/dependency-injection) veri tüm kullanıcılar tarafından kullanılabilmesini sağlamak için:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-252">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="9e9ec-253">Veri içeren bir hizmet tanımlama (örneğin, adlı bir sınıf `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-253">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="9e9ec-254">Hizmet sınıfına ekleyin `ConfigureServices` (örneğin `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-254">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="9e9ec-255">Her denetleyici veri hizmeti sınıfında kullanılmasına neden:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-255">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="9e9ec-256">Oturumla çalışırken sık karşılaşılan hataları</span><span class="sxs-lookup"><span data-stu-id="9e9ec-256">Common errors when working with session</span></span>

* <span data-ttu-id="9e9ec-257">"Çözümlenemiyor hizmet türü 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' için 'Microsoft.AspNetCore.Session.DistributedSessionStore' etkinleştirmeye çalışırken."</span><span class="sxs-lookup"><span data-stu-id="9e9ec-257">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="9e9ec-258">En az bir yapılandırmak devrederek nedeni genellikle `IDistributedCache` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-258">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="9e9ec-259">Daha fazla bilgi için bkz: [dağıtılmış bir önbellekle çalışmaya](xref:performance/caching/distributed) ve [bellek önbelleğe alma işleminde](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="9e9ec-259">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="9e9ec-260">Ara yazılım başarısız için oturum oturumu kalıcı olması durumunda (örneğin: veritabanı kullanılabilir durumda değilse), özel durumu günlüğe kaydeder ve onu yuttuğu.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-260">In the event that the session middleware fails to persist a session (for example: if the database isn't available), it logs the exception and swallows it.</span></span> <span data-ttu-id="9e9ec-261">İstek daha sonra normal olarak, hangi çok beklenmeyen davranışlara müşteri adayları devam eder.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-261">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="9e9ec-262">Tipik bir örnek:</span><span class="sxs-lookup"><span data-stu-id="9e9ec-262">A typical example:</span></span>

<span data-ttu-id="9e9ec-263">Birisi bir alışveriş sepeti oturumunda depolar.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-263">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="9e9ec-264">Kullanıcı bir öğe ekler, ancak yürütme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-264">The user adds an item but the commit fails.</span></span> <span data-ttu-id="9e9ec-265">"Öğesi, hangi true değil eklendi" iletisi raporları için uygulama hatası hakkında bilmiyor.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-265">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="9e9ec-266">Bu tür hataları denetlemek için önerilen yol çağırmaktır `await feature.Session.CommitAsync();` tamamladığınızda uygulama kodundan oturuma yazma.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-266">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="9e9ec-267">Sonra şu hata ile şeyleri yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-267">Then you can do what you like with the error.</span></span> <span data-ttu-id="9e9ec-268">Çağrılırken aynı şekilde çalışır `LoadAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e9ec-268">It works the same way when calling `LoadAsync`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="9e9ec-269">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9e9ec-269">Additional resources</span></span>

* [<span data-ttu-id="9e9ec-270">ASP.NET Core 1.x: Bu belgede kullanılan kod örneği</span><span class="sxs-lookup"><span data-stu-id="9e9ec-270">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="9e9ec-271">ASP.NET Core 2.x: Bu belgede kullanılan kod örneği</span><span class="sxs-lookup"><span data-stu-id="9e9ec-271">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
