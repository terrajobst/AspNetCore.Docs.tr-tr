---
title: ASP.NET core'da oturum ve uygulama durumu
author: rick-anderson
description: İstekleri arasında oturum ve uygulama durumunu korumak için yaklaşımları keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: ccaaa6fafd611c3cf35a9171d5bfd6100535eeb9
ms.sourcegitcommit: 0fc89b80bb1952852ecbcf3c5c156459b02a6ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52618135"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="4acb2-103">ASP.NET core'da oturum ve uygulama durumu</span><span class="sxs-lookup"><span data-stu-id="4acb2-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="4acb2-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4acb2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4acb2-105">HTTP durum bilgisi olmayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="4acb2-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="4acb2-106">Ek adımlar yapmadan kullanıcı değerleri veya uygulama durumunu tutmaz bağımsız mesaj HTTP isteği adı verilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="4acb2-107">Bu makalede, istekler arasında kullanıcı verileri ve uygulama durumunu korumak için çeşitli yaklaşımlar açıklanır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="4acb2-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4acb2-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="4acb2-109">Durum Yönetimi</span><span class="sxs-lookup"><span data-stu-id="4acb2-109">State management</span></span>

<span data-ttu-id="4acb2-110">Durum, çeşitli yaklaşımlar kullanılarak depolanabilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-110">State can be stored using several approaches.</span></span> <span data-ttu-id="4acb2-111">Her bir yaklaşım, bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="4acb2-112">Depolama yaklaşımı</span><span class="sxs-lookup"><span data-stu-id="4acb2-112">Storage approach</span></span> | <span data-ttu-id="4acb2-113">Depolama mekanizması</span><span class="sxs-lookup"><span data-stu-id="4acb2-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="4acb2-114">Çerezler</span><span class="sxs-lookup"><span data-stu-id="4acb2-114">Cookies</span></span>](#cookies) | <span data-ttu-id="4acb2-115">HTTP tanımlama bilgileri (sunucu tarafı uygulama kodu kullanarak depolanan veriler dahil)</span><span class="sxs-lookup"><span data-stu-id="4acb2-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="4acb2-116">Oturum durumu</span><span class="sxs-lookup"><span data-stu-id="4acb2-116">Session state</span></span>](#session-state) | <span data-ttu-id="4acb2-117">HTTP tanımlama bilgileri ve sunucu tarafı uygulama kodu</span><span class="sxs-lookup"><span data-stu-id="4acb2-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="4acb2-118">TempData</span><span class="sxs-lookup"><span data-stu-id="4acb2-118">TempData</span></span>](#tempdata) | <span data-ttu-id="4acb2-119">HTTP tanımlama bilgileri veya oturum durumu</span><span class="sxs-lookup"><span data-stu-id="4acb2-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="4acb2-120">Sorgu dizeleri</span><span class="sxs-lookup"><span data-stu-id="4acb2-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="4acb2-121">HTTP sorgu dizeleri</span><span class="sxs-lookup"><span data-stu-id="4acb2-121">HTTP query strings</span></span> |
| [<span data-ttu-id="4acb2-122">Gizli alanları</span><span class="sxs-lookup"><span data-stu-id="4acb2-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="4acb2-123">HTTP form alanları</span><span class="sxs-lookup"><span data-stu-id="4acb2-123">HTTP form fields</span></span> |
| [<span data-ttu-id="4acb2-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="4acb2-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="4acb2-125">Sunucu tarafı uygulama kodu</span><span class="sxs-lookup"><span data-stu-id="4acb2-125">Server-side app code</span></span> |
| [<span data-ttu-id="4acb2-126">Önbellek</span><span class="sxs-lookup"><span data-stu-id="4acb2-126">Cache</span></span>](#cache) | <span data-ttu-id="4acb2-127">Sunucu tarafı uygulama kodu</span><span class="sxs-lookup"><span data-stu-id="4acb2-127">Server-side app code</span></span> |
| [<span data-ttu-id="4acb2-128">Bağımlılık Ekleme</span><span class="sxs-lookup"><span data-stu-id="4acb2-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="4acb2-129">Sunucu tarafı uygulama kodu</span><span class="sxs-lookup"><span data-stu-id="4acb2-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="4acb2-130">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="4acb2-130">Cookies</span></span>

<span data-ttu-id="4acb2-131">Tanımlama bilgileri, istekler arasında verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-131">Cookies store data across requests.</span></span> <span data-ttu-id="4acb2-132">Tanımlama bilgileri her bir istekle gönderildiğinden, bunların boyutu için en az tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="4acb2-133">İdeal olarak, yalnızca bir tanımlayıcı bir tanımlama bilgisinde, uygulama tarafından depolanan verilerle depolanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="4acb2-134">Tarayıcılarının çoğu tanımlama bilgisi boyutu 4096 bayt ile sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="4acb2-135">Yalnızca sınırlı sayıda tanımlama bilgileri, her etki alanı için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="4acb2-136">Tanımlama bilgilerini oynama tabi olduğu için uygulama tarafından doğrulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="4acb2-137">Tanımlama bilgileri, kullanıcılar tarafından silinebilir ve istemcilerde süresi dolar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="4acb2-138">Ancak, tanımlama bilgileri genellikle en sağlam istemci üzerindeki veri kalıcılığı biçimindedir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="4acb2-139">Tanımlama bilgileri, genellikle içeriği bilinen bir kullanıcı için burada özelleştirilmiş kişiselleştirme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="4acb2-140">Kullanıcı yalnızca tanımlanan ve çoğu zaman kimliği doğrulanmış değil.</span><span class="sxs-lookup"><span data-stu-id="4acb2-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="4acb2-141">Tanımlama bilgisi, kullanıcının adı, hesap adı veya benzersiz kullanıcı kimliği (örneğin, bir GUID) depolayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4acb2-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="4acb2-142">Tanımlama bilgisi ardından kendi tercih edilen Web sitesi arka plan rengi gibi kullanıcının kişiselleştirilmiş ayarlara erişmek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4acb2-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="4acb2-143">Dikkatli olmanızı [Avrupa Birliği genel veri koruma yönetmeliklerine (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) tanımlama bilgileri vermek ve gizlilikle ilgili ne zaman ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="4acb2-144">Daha fazla bilgi için [ASP.NET Core genel veri koruma yönetmeliği (GDPR) desteği](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="4acb2-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="4acb2-145">Oturum durumu</span><span class="sxs-lookup"><span data-stu-id="4acb2-145">Session state</span></span>

<span data-ttu-id="4acb2-146">Kullanıcının bir web uygulaması gözatar sırada oturum durumu kullanıcı verilerinin depolanması için bir ASP.NET Core senaryo ' dir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="4acb2-147">Oturum durumu, istemciden gelen isteklere arasında verileri kalıcı hale getirmek için uygulama tarafından korunan bir depolama kullanır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="4acb2-148">Oturum verilerini bir önbelleği tarafından desteklenen ve kısa ömürlü veri kabul&mdash;site oturum verileri çalışmaya devam etmelidir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span>

> [!NOTE]
> <span data-ttu-id="4acb2-149">Oturum desteklenmiyor [SignalR](xref:signalr/index) uygulamaları çünkü bir [SignalR hub'ı](xref:signalr/hubs) bağımsız bir HTTP bağlamını yürütür.</span><span class="sxs-lookup"><span data-stu-id="4acb2-149">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="4acb2-150">Örneğin, bir uzun olduğunda ortaya çıkabilir yoklama isteği açık tarafından tutulan isteğin HTTP bağlamını ömrünü ötesinde bir hub'ı.</span><span class="sxs-lookup"><span data-stu-id="4acb2-150">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="4acb2-151">ASP.NET Core, uygulamanın her istek ile gönderilen bir oturum kimliği içeren istemciye bir tanımlama bilgisi sağlayarak oturum durumu korur.</span><span class="sxs-lookup"><span data-stu-id="4acb2-151">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="4acb2-152">Uygulama, oturum verilerini almak için oturum kimliği kullanır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-152">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="4acb2-153">Oturum durumu aşağıdaki davranışları sergileyen:</span><span class="sxs-lookup"><span data-stu-id="4acb2-153">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="4acb2-154">Oturum tanımlama bilgisinin tarayıcıya belirli olduğundan, oturumları tarayıcılar arasında paylaşılmaz.</span><span class="sxs-lookup"><span data-stu-id="4acb2-154">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="4acb2-155">Tarayıcı oturumu sona erdiğinde oturum tanımlama bilgileri silinir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-155">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="4acb2-156">Süresi dolmuş bir oturum için bir tanımlama bilgisi alınmazsa, aynı oturum tanımlama bilgisini kullanan yeni bir oturum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4acb2-156">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="4acb2-157">Boş oturumlar olmayan korunur&mdash;oturum içine istekler genelinde oturumu kalıcı olarak en az bir değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-157">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="4acb2-158">Bir oturum korunmaz, yeni oturum kimliği her yeni isteği için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4acb2-158">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="4acb2-159">Uygulamayı sınırlı bir süre sonra son istek için bir oturum korur.</span><span class="sxs-lookup"><span data-stu-id="4acb2-159">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="4acb2-160">Uygulama oturum zaman aşımı ayarlar veya 20 dakikalık varsayılan değeri kullanır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-160">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="4acb2-161">Oturum durumu, belirli bir oturum için özeldir ancak burada veri oturumlarda kalıcı depolama alanı gerektirmez kullanıcı verilerini depolamak için idealdir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-161">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="4acb2-162">Oturum verileri silinir ya da [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) oturum süresinin sona erdiği veya uygulama çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-162">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="4acb2-163">Uygulama kodu istemci tarayıcısı kapatıldı veya ne zaman oturum tanımlama bilgisi silindi veya istemcide süresi size bildirmek için varsayılan bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="4acb2-163">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>

> [!WARNING]
> <span data-ttu-id="4acb2-164">Hassas verileri kavramak depolamayın.</span><span class="sxs-lookup"><span data-stu-id="4acb2-164">Don't store sensitive data in session state.</span></span> <span data-ttu-id="4acb2-165">Kullanıcı olmayan Tarayıcıyı kapatın ve oturum tanımlama bilgisini temizlemek.</span><span class="sxs-lookup"><span data-stu-id="4acb2-165">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="4acb2-166">Bazı tarayıcılar tarayıcı pencereleri arasında geçerli bir oturum tanımlama bilgileri korur.</span><span class="sxs-lookup"><span data-stu-id="4acb2-166">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="4acb2-167">Bir oturum için tek bir kullanıcı kısıtlı olmayabilir&mdash;sonraki kullanıcı aynı oturum tanımlama bilgisinin uygulamayla göz atmak devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-167">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="4acb2-168">Bellek içi önbelleği sağlayıcısı uygulama bulunduğu sunucunun bellekte oturum verilerini depolar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-168">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="4acb2-169">Bir sunucu grubu senaryoda:</span><span class="sxs-lookup"><span data-stu-id="4acb2-169">In a server farm scenario:</span></span>

* <span data-ttu-id="4acb2-170">Kullanım *Yapışkan oturumlar* tek bir sunucu üzerinde her oturum belirli uygulama örneğine bağlamak için.</span><span class="sxs-lookup"><span data-stu-id="4acb2-170">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="4acb2-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) kullanan [uygulama isteği yönlendirme (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) Yapışkan oturumlar varsayılan olarak zorunlu kılmak için.</span><span class="sxs-lookup"><span data-stu-id="4acb2-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="4acb2-172">Ancak, Yapışkan oturumlar ölçeklenebilirliği etkileyebilir ve web uygulama güncelleştirmeleri zorlaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-172">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="4acb2-173">Bir Redis veya SQL Server kullanmak için daha iyi bir yaklaşım olan dağıtılmış önbellek, Yapışkan oturumlar gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="4acb2-173">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="4acb2-174">Daha fazla bilgi için bkz. <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="4acb2-174">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="4acb2-175">Oturum tanımlama bilgisi ile şifrelenmiş [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span><span class="sxs-lookup"><span data-stu-id="4acb2-175">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="4acb2-176">Veri koruma, her bir makinede oturum tanımlama bilgileri okumak için düzgün şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-176">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="4acb2-177">Daha fazla bilgi için <xref:security/data-protection/introduction> ve [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="4acb2-177">For more information, see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="4acb2-178">Oturum durumunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="4acb2-178">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4acb2-179">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) dahil edilen paket [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), oturum durumu yönetmek için bir ara yazılım sağlar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-179">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="4acb2-180">Oturum ara yazılım etkinleştirmek için `Startup` içermelidir:</span><span class="sxs-lookup"><span data-stu-id="4acb2-180">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4acb2-181">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) paket, oturum durumu yönetmek için bir ara yazılım sağlar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-181">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="4acb2-182">Oturum ara yazılım etkinleştirmek için `Startup` içermelidir:</span><span class="sxs-lookup"><span data-stu-id="4acb2-182">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="4acb2-183">Herhangi bir [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) bellek önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-183">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="4acb2-184">`IDistributedCache` Uygulama oturumu için bir yedekleme deposu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-184">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="4acb2-185">Daha fazla bilgi için bkz. <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="4acb2-185">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="4acb2-186">Bir çağrı [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) içinde `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4acb2-186">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="4acb2-187">Bir çağrı [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) içinde `Configure`.</span><span class="sxs-lookup"><span data-stu-id="4acb2-187">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="4acb2-188">Aşağıdaki kod, varsayılan bir bellek içi uygulama ile bellek içi oturum sağlayıcısını ayarlama işlemi gösterilmektedir `IDistributedCache`:</span><span class="sxs-lookup"><span data-stu-id="4acb2-188">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

<span data-ttu-id="4acb2-189">Ara yazılım sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-189">The order of middleware is important.</span></span> <span data-ttu-id="4acb2-190">Yukarıdaki örnekte, bir `InvalidOperationException` özel durum oluşursa, `UseSession` sonra çağrılan `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="4acb2-190">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="4acb2-191">Daha fazla bilgi için [ara yazılım sıralama](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="4acb2-191">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

<span data-ttu-id="4acb2-192">[İçeriğin HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) oturum durumu yapılandırıldıktan sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-192">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="4acb2-193">`HttpContext.Session` önce erişilemez `UseSession` çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="4acb2-193">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="4acb2-194">Uygulama yanıt akışına yazma başladıktan sonra yeni oturum tanımlama bilgisi ile yeni bir oturum oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="4acb2-194">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="4acb2-195">Özel durum web sunucusu günlüğe kaydedilen ve tarayıcıda gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="4acb2-195">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="4acb2-196">Oturum durumunu zaman uyumsuz olarak yükleme</span><span class="sxs-lookup"><span data-stu-id="4acb2-196">Load session state asynchronously</span></span>

<span data-ttu-id="4acb2-197">Temel oturum kayıtları ASP.NET Core varsayılan oturum sağlayıcısında yükler [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) yedekleme deposu zaman uyumsuz olarak yalnızca şu durumlarda [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) yöntemi açıkça çağrılır önce [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [ayarlamak](/dotnet/api/microsoft.aspnetcore.http.isession.set), veya [Kaldır](/dotnet/api/microsoft.aspnetcore.http.isession.remove) yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="4acb2-197">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="4acb2-198">Varsa `LoadAsync` ilk olarak, temel olarak adlandırılmaz oturumu kayıt yüklendiği zaman uyumlu olarak, hangi ölçekte bir performans cezasına sebep.</span><span class="sxs-lookup"><span data-stu-id="4acb2-198">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="4acb2-199">Bu düzen zorunlu uygulamanız için sarmalamanız [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) ve [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) uygulamaları bağlanamazsa özel durum sürümleri ile `LoadAsync` değil yönteminden önce `TryGetValue`, `Set`, veya `Remove`.</span><span class="sxs-lookup"><span data-stu-id="4acb2-199">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="4acb2-200">Sarmalanan sürümleri Hizmetleri kapsayıcısında kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4acb2-200">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="4acb2-201">Oturum seçenekleri</span><span class="sxs-lookup"><span data-stu-id="4acb2-201">Session options</span></span>

<span data-ttu-id="4acb2-202">Oturum Varsayılanları geçersiz kılmak için kullanın [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span><span class="sxs-lookup"><span data-stu-id="4acb2-202">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="4acb2-203">Seçenek</span><span class="sxs-lookup"><span data-stu-id="4acb2-203">Option</span></span> | <span data-ttu-id="4acb2-204">Açıklama</span><span class="sxs-lookup"><span data-stu-id="4acb2-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="4acb2-205">Tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="4acb2-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="4acb2-206">Tanımlama bilgisi oluşturmak için kullanılan ayarları belirler.</span><span class="sxs-lookup"><span data-stu-id="4acb2-206">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="4acb2-207">[Adı](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) varsayılan olarak [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="4acb2-207">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="4acb2-208">[Yol](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) varsayılan olarak [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="4acb2-208">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="4acb2-209">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) varsayılan olarak [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span><span class="sxs-lookup"><span data-stu-id="4acb2-209">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="4acb2-210">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="4acb2-210">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="4acb2-211">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="4acb2-211">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="4acb2-212">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="4acb2-212">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="4acb2-213">`IdleTimeout` İçeriğini terk önce ne kadar süreyle oturumun boşta kalabileceği gösterir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-213">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="4acb2-214">Her oturum erişimi zaman aşımı sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-214">Each session access resets the timeout.</span></span> <span data-ttu-id="4acb2-215">Bu ayar, yalnızca oturum tanımlama içerik için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-215">This setting only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="4acb2-216">Varsayılan değer 20 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-216">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="4acb2-217">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="4acb2-217">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="4acb2-218">En yüksek süreyi Mağaza'dan oturum yüklemek veya geri depoya kaydetmeye izin.</span><span class="sxs-lookup"><span data-stu-id="4acb2-218">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="4acb2-219">Bu ayar, yalnızca zaman uyumsuz işlemler için geçerli olabilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-219">This setting may only apply to asynchronous operations.</span></span> <span data-ttu-id="4acb2-220">Bu zaman aşımı kullanarak devre dışı bırakılabilir [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span><span class="sxs-lookup"><span data-stu-id="4acb2-220">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="4acb2-221">Varsayılan değer 1 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-221">The default is 1 minute.</span></span> |

<span data-ttu-id="4acb2-222">Oturum tanımlama bilgisi istekleri tek bir tarayıcıdan belirlemek ve izlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="4acb2-223">Varsayılan olarak, bu tanımlama bilgisi adlı `.AspNetCore.Session`, ve bir yol kullanır `/`.</span><span class="sxs-lookup"><span data-stu-id="4acb2-223">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="4acb2-224">Tanımlama bilgisinin varsayılan bir etki alanını belirtmeyen çünkü, istemci tarafı komut dosyası için sayfada kullanılabilir değil (çünkü [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) varsayılan olarak `true`).</span><span class="sxs-lookup"><span data-stu-id="4acb2-224">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="4acb2-225">Seçenek</span><span class="sxs-lookup"><span data-stu-id="4acb2-225">Option</span></span> | <span data-ttu-id="4acb2-226">Açıklama</span><span class="sxs-lookup"><span data-stu-id="4acb2-226">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="4acb2-227">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="4acb2-227">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="4acb2-228">Tanımlama bilgisi oluşturmak için kullanılan etki alanını belirler.</span><span class="sxs-lookup"><span data-stu-id="4acb2-228">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="4acb2-229">`CookieDomain` Varsayılan olarak ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="4acb2-229">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="4acb2-230">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="4acb2-230">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="4acb2-231">Tarayıcı tanımlama bilgisine istemci tarafı JavaScript tarafından erişilecek izin veriyorsa belirler.</span><span class="sxs-lookup"><span data-stu-id="4acb2-231">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="4acb2-232">Varsayılan `true`, tanımlama bilgisi başka bir deyişle, yalnızca HTTP isteklerine geçirilir ve sayfaya betiğe kullanılabilir hale değil.</span><span class="sxs-lookup"><span data-stu-id="4acb2-232">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="4acb2-233">CookieName</span><span class="sxs-lookup"><span data-stu-id="4acb2-233">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="4acb2-234">Oturum kimliği kalıcı hale getirmek için kullanılan tanımlama bilgisi adını belirler</span><span class="sxs-lookup"><span data-stu-id="4acb2-234">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="4acb2-235">Varsayılan değer [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="4acb2-235">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="4acb2-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="4acb2-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="4acb2-237">Tanımlama bilgisi oluşturmak için kullanılan yolu belirler.</span><span class="sxs-lookup"><span data-stu-id="4acb2-237">Determines the path used to create the cookie.</span></span> <span data-ttu-id="4acb2-238">Varsayılan olarak [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="4acb2-238">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="4acb2-239">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="4acb2-239">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="4acb2-240">Tanımlama bilgisinin yalnızca HTTPS isteklerini iletilip iletilmeyeceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="4acb2-240">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="4acb2-241">Varsayılan değer [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="4acb2-241">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="4acb2-242">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="4acb2-242">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="4acb2-243">`IdleTimeout` İçeriğini terk önce ne kadar süreyle oturumun boşta kalabileceği gösterir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-243">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="4acb2-244">Her oturum erişimi zaman aşımı sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-244">Each session access resets the timeout.</span></span> <span data-ttu-id="4acb2-245">Bu, yalnızca oturum tanımlama içeriği geçerlidir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4acb2-245">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="4acb2-246">Varsayılan değer 20 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-246">The default is 20 minutes.</span></span> |

<span data-ttu-id="4acb2-247">Oturum tanımlama bilgisi istekleri tek bir tarayıcıdan belirlemek ve izlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-247">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="4acb2-248">Varsayılan olarak, bu tanımlama bilgisi adlı `.AspNet.Session`, ve bir yol kullanır `/`.</span><span class="sxs-lookup"><span data-stu-id="4acb2-248">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="4acb2-249">Tanımlama bilgisi oturumu Varsayılanları geçersiz kılmak için kullanın `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="4acb2-249">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

<span data-ttu-id="4acb2-250">Uygulamanın kullandığı [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) sunucusunun önbelleğine içeriğini terk önce ne kadar bir oturumun boşta kalabileceği belirlemek için özellik.</span><span class="sxs-lookup"><span data-stu-id="4acb2-250">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="4acb2-251">Bu özellik, tanımlama bilgisi süre sonu bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-251">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="4acb2-252">Geçtiği her isteğin [oturumu ara yazılım](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) zaman aşımı sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-252">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="4acb2-253">Oturum durumu *kilitlenmeyen*.</span><span class="sxs-lookup"><span data-stu-id="4acb2-253">Session state is *non-locking*.</span></span> <span data-ttu-id="4acb2-254">Son istek, iki isteği aynı anda bir oturumu içeriğini değiştirme girişimi, ilk geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-254">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="4acb2-255">`Session` olarak uygulanan bir *tutarlı oturumu*, yani tüm içeriği birlikte depolanır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-255">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="4acb2-256">Farklı oturuma değerlerini değiştirmek iki istek arama, son istek oturumu değişikliklerinin ilk tarafından geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-256">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="4acb2-257">Ayarlayın ve oturumu değerlerini alma</span><span class="sxs-lookup"><span data-stu-id="4acb2-257">Set and get Session values</span></span>

<span data-ttu-id="4acb2-258">Oturum durumu, bir Razor sayfaları erişildiğinde [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) sınıfı veya MVC [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller) sınıfıyla [içeriğin HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="4acb2-258">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="4acb2-259">Bu özellik bir [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="4acb2-259">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4acb2-260">`ISession` Uygulamasını tamsayı ve dize değerleri ayarlama ve alma için birkaç genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-260">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="4acb2-261">Uzantı yöntemleri [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) ad alanı (ekleme bir `using Microsoft.AspNetCore.Http;` genişletme yöntemleri erişim kazanmak için deyimi) olduğunda [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) Paket proje tarafından başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="4acb2-261">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="4acb2-262">Her iki paketi de dahil edilen [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4acb2-262">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4acb2-263">`ISession` Uygulamasını tamsayı ve dize değerleri ayarlama ve alma için birkaç genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-263">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="4acb2-264">Uzantı yöntemleri [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) ad alanı (ekleme bir `using Microsoft.AspNetCore.Http;` genişletme yöntemleri erişim kazanmak için deyimi) olduğunda [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) Paket proje tarafından başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="4acb2-264">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="4acb2-265">`ISession` Uzantı yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="4acb2-265">`ISession` extension methods:</span></span>

* [<span data-ttu-id="4acb2-266">Get (ISession, String)</span><span class="sxs-lookup"><span data-stu-id="4acb2-266">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="4acb2-267">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="4acb2-267">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="4acb2-268">GetString (ISession, String)</span><span class="sxs-lookup"><span data-stu-id="4acb2-268">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="4acb2-269">Setınt32 (ISession, dize, Int32)</span><span class="sxs-lookup"><span data-stu-id="4acb2-269">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="4acb2-270">SetString (ISession, String, String)</span><span class="sxs-lookup"><span data-stu-id="4acb2-270">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="4acb2-271">Aşağıdaki örnekte oturum değerini alır. `IndexModel.SessionKeyName` anahtarı (`_Name` örnek uygulamada) bir Razor sayfaları sayfasında:</span><span class="sxs-lookup"><span data-stu-id="4acb2-271">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="4acb2-272">Aşağıdaki örnek, tamsayı ve bir dize almak ve ayarlamak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4acb2-272">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

<span data-ttu-id="4acb2-273">Bellek içi önbelleği kullanırken bile bir dağıtılmış önbellek senaryoyu etkinleştirmek için tüm oturum verilerini seri hale getirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-273">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="4acb2-274">En az dize ve sayı seri hale getiricileri genişletme sağlanır (uzantı yöntemleri ve yöntemler bkz [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="4acb2-274">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="4acb2-275">JSON gibi başka bir mekanizma kullanarak kullanıcı tarafından karmaşık türler seri hale getirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-275">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="4acb2-276">Serileştirilebilir nesneler almak ve ayarlamak için aşağıdaki uzantı yöntemlerini ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4acb2-276">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4acb2-277">Aşağıdaki örnek, genişletme yöntemleri ile seri hale getirilebilir bir nesne almak ve ayarlamak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="4acb2-277">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="4acb2-278">TempData</span><span class="sxs-lookup"><span data-stu-id="4acb2-278">TempData</span></span>

<span data-ttu-id="4acb2-279">ASP.NET Core sunan [Razor sayfaları sayfa modeli TempData özelliği](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) veya [MVC denetleyicisinin TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span><span class="sxs-lookup"><span data-stu-id="4acb2-279">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="4acb2-280">Bu özellik, okunur kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-280">This property stores data until it's read.</span></span> <span data-ttu-id="4acb2-281">[Tutmak](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) ve [Özet](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) yöntemleri silme olmadan verileri incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-281">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="4acb2-282">Veriler birden çok tek bir istek için gerekli olduğunda TempData yeniden yönlendirme için özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-282">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="4acb2-283">TempData tanımlama veya oturum durumu kullanarak TempData sağlayıcıları tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-283">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="4acb2-284">TempData sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="4acb2-284">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4acb2-285">ASP.NET Core 2.0 veya sonraki sürümlerde, tanımlama bilgisi tabanlı TempData sağlayıcısı TempData tanımlama bilgilerinizde depolamak için varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-285">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="4acb2-286">Tanımlama bilgisi verisi kullanılarak şifrelenir [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), ile kodlanmış [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), sonra parçalı.</span><span class="sxs-lookup"><span data-stu-id="4acb2-286">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="4acb2-287">Tanımlama bilgisi öbekli çünkü tek bir tanımlama bilgisi ASP.NET Core 1.x uygulanmaz bulunan sınırı boyutu.</span><span class="sxs-lookup"><span data-stu-id="4acb2-287">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="4acb2-288">Şifrelenmiş verileri sıkıştırmak için güvenlik sorunları gibi neden olabileceği için tanımlama bilgisi verisi sıkıştırılmaz [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlal](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="4acb2-288">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="4acb2-289">Tanımlama bilgisi tabanlı TempData sağlayıcısı hakkında daha fazla bilgi için bkz. [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="4acb2-289">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4acb2-290">ASP.NET Core 1.0 ve 1.1, oturum durumu TempData sağlayıcısı varsayılan sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-290">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="4acb2-291">TempData sağlayıcısını seçin</span><span class="sxs-lookup"><span data-stu-id="4acb2-291">Choose a TempData provider</span></span>

<span data-ttu-id="4acb2-292">TempData sağlayıcısı seçme gibi çeşitli konuları içerir:</span><span class="sxs-lookup"><span data-stu-id="4acb2-292">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="4acb2-293">Uygulama zaten oturum durumu kullanıyor mu?</span><span class="sxs-lookup"><span data-stu-id="4acb2-293">Does the app already use session state?</span></span> <span data-ttu-id="4acb2-294">Bu durumda, oturum durumu TempData sağlayıcısını kullanarak ek ücret ödemeden (yanı sıra veri boyutu) uygulamasına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-294">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="4acb2-295">Uygulamayı yalnızca gelişigüzel TempData kullanmaz için görece küçük miktarlarda veri (bayt cinsinden en fazla 500)?</span><span class="sxs-lookup"><span data-stu-id="4acb2-295">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="4acb2-296">Bu nedenle, tanımlama bilgisi TempData sağlayıcı her istek için küçük bir maliyet eklerse, TempData taşır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-296">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="4acb2-297">Aksi durumda, oturum durumu TempData sağlayıcısı TempData tüketilen kadar gidiş dönüşü her isteğin verilerde büyük miktarda önlemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-297">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="4acb2-298">Uygulamayı bir sunucu grubunda birden çok sunucu üzerinde çalışıyor mu?</span><span class="sxs-lookup"><span data-stu-id="4acb2-298">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="4acb2-299">Bu nedenle, varsa tanımlama bilgisi TempData sağlayıcısı veri koruma dışında kullanmak için ek yapılandırma (bkz <xref:security/data-protection/introduction> ve [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers)).</span><span class="sxs-lookup"><span data-stu-id="4acb2-299">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="4acb2-300">Çoğu web istemcileri (örneğin, web tarayıcıları) her bir tanımlama bilgisi, tanımlama bilgilerinin toplam sayısı veya her ikisi de en büyük boyutu üst sınırı uygular.</span><span class="sxs-lookup"><span data-stu-id="4acb2-300">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="4acb2-301">Tanımlama bilgisi TempData sağlayıcı kullanırken, uygulama bu sınırları aşmanız olmaz doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="4acb2-301">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="4acb2-302">Verilerin toplam boyutuna göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="4acb2-302">Consider the total size of the data.</span></span> <span data-ttu-id="4acb2-303">Hesap için şifreleme ve Öbekleme nedeniyle tanımlama bilgisi boyutu artar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-303">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="4acb2-304">TempData sağlayıcısını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4acb2-304">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4acb2-305">Tanımlama bilgisi tabanlı TempData sağlayıcısı varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-305">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="4acb2-306">Oturum tabanlı TempData sağlayıcıyı etkinleştirmek için [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4acb2-306">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4acb2-307">Aşağıdaki `Startup` sınıf kodunu oturum tabanlı TempData sağlayıcısı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="4acb2-307">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

<span data-ttu-id="4acb2-308">Ara yazılım sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-308">The order of middleware is important.</span></span> <span data-ttu-id="4acb2-309">Yukarıdaki örnekte, bir `InvalidOperationException` özel durum oluşursa, `UseSession` sonra çağrılan `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="4acb2-309">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="4acb2-310">Daha fazla bilgi için [ara yazılım sıralama](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="4acb2-310">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4acb2-311">.NET Framework'ü hedefleyen ve oturum tabanlı TempData sağlayıcısını kullanarak eklerseniz [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) projeye paket.</span><span class="sxs-lookup"><span data-stu-id="4acb2-311">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="4acb2-312">Sorgu dizeleri</span><span class="sxs-lookup"><span data-stu-id="4acb2-312">Query strings</span></span>

<span data-ttu-id="4acb2-313">Sınırlı miktarda veri bir istekten başka bir yeni isteğin sorgu dizesi olanağı ekleyerek geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-313">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="4acb2-314">Bu, gömülü durumuyla e-posta veya sosyal ağlar paylaşılan bağlantılara izin verir, kalıcı bir şekilde durumu yakalamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-314">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="4acb2-315">Hiçbir zaman URL'si sorgu dizeleri ortak olduğundan, hassas verileri için sorgu dizelerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4acb2-315">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="4acb2-316">İstenmeyen paylaşmaya ek olarak, sorgu dizelerine veriler dahil olmak üzere olanaklarını oluşturabilirsiniz [siteler arası istek sahteciliği (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) kullanıcıların kimlik doğrulaması sırasında kötü amaçlı siteleri ziyaret etmeye ikna saldırıları.</span><span class="sxs-lookup"><span data-stu-id="4acb2-316">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="4acb2-317">Saldırganların uygulamadaki kullanıcı verilerini çalabilir veya kullanıcı adına kötü amaçlı eylemler.</span><span class="sxs-lookup"><span data-stu-id="4acb2-317">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="4acb2-318">Herhangi bir korunan bir uygulama veya oturum durumu CSRF saldırılarına karşı korumanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-318">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="4acb2-319">Daha fazla bilgi için [önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="4acb2-319">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="4acb2-320">Gizli alanları</span><span class="sxs-lookup"><span data-stu-id="4acb2-320">Hidden fields</span></span>

<span data-ttu-id="4acb2-321">Veri gizli form alanları kaydedilebilir ve bir sonraki istekte geri gönderildi.</span><span class="sxs-lookup"><span data-stu-id="4acb2-321">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="4acb2-322">Bu, çok sayfalı formlarında yaygındır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-322">This is common in multi-page forms.</span></span> <span data-ttu-id="4acb2-323">İstemci, olası verilerle değiştirmesine çünkü uygulama gizli alanlar depolanan veriler her zaman düzeltin gerekir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-323">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="4acb2-324">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="4acb2-324">HttpContext.Items</span></span>

<span data-ttu-id="4acb2-325">[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) koleksiyonu tek bir isteği işlerken veri depolamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-325">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="4acb2-326">İstek işlendikten sonra koleksiyonun içeriği atılır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-326">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="4acb2-327">`Items` Bileşenleri veya ara yazılım istek sırada, farklı noktalarda çalışır ve parametreleri geçirmek için doğrudan hiçbir şekilde iletişim kurmasına izin vermek için genellikle koleksiyonu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4acb2-327">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="4acb2-328">Aşağıdaki örnekte, [ara yazılım](xref:fundamentals/middleware/index) ekler `isVerified` için `Items` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="4acb2-328">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="4acb2-329">İşlem hattı daha sonra başka bir ara yazılım değerini erişip `isVerified`:</span><span class="sxs-lookup"><span data-stu-id="4acb2-329">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="4acb2-330">Yalnızca tek bir uygulama tarafından kullanılan ara yazılımı `string` anahtarları kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-330">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="4acb2-331">Uygulama örnekleri arasında paylaşılan bir ara yazılım, anahtar çarpışmalarını için benzersiz nesne anahtarları kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-331">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="4acb2-332">Aşağıdaki örnek, bir ara yazılım sınıfı içinde tanımlanan bir benzersiz nesne anahtarı kullanma işlemini gösterir:</span><span class="sxs-lookup"><span data-stu-id="4acb2-332">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

<span data-ttu-id="4acb2-333">Diğer kod içinde depolanan değeri erişip `HttpContext.Items` ara yazılım sınıfı tarafından kullanıma sunulan anahtarını kullanarak:</span><span class="sxs-lookup"><span data-stu-id="4acb2-333">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="4acb2-334">Bu yaklaşım, aynı zamanda koddaki anahtar dizeleri kullanımı ortadan avantajına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-334">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="4acb2-335">Önbellek</span><span class="sxs-lookup"><span data-stu-id="4acb2-335">Cache</span></span>

<span data-ttu-id="4acb2-336">Önbelleğe alma, veri depolayıp almak için etkili bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="4acb2-336">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="4acb2-337">Uygulama, önbelleğe alınmış öğeleri ömrünü kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4acb2-337">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="4acb2-338">Önbelleğe alınan veriler belirli bir istek, kullanıcı veya oturum ile ilişkili değil.</span><span class="sxs-lookup"><span data-stu-id="4acb2-338">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="4acb2-339">**Dikkatli olun diğer kullanıcıların isteklerinden alınan önbellek kullanıcıya özgü verilere değil.**</span><span class="sxs-lookup"><span data-stu-id="4acb2-339">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="4acb2-340">Daha fazla bilgi için bkz. <xref:performance/caching/response>.</span><span class="sxs-lookup"><span data-stu-id="4acb2-340">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="4acb2-341">Bağımlılık Ekleme</span><span class="sxs-lookup"><span data-stu-id="4acb2-341">Dependency Injection</span></span>

<span data-ttu-id="4acb2-342">Kullanım [bağımlılık ekleme](xref:fundamentals/dependency-injection) veri tüm kullanıcılar için kullanılabilir hale getirmek için:</span><span class="sxs-lookup"><span data-stu-id="4acb2-342">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="4acb2-343">Veri içeren bir hizmet tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-343">Define a service containing the data.</span></span> <span data-ttu-id="4acb2-344">Örneğin, adında bir sınıf `MyAppData` tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="4acb2-344">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="4acb2-345">Hizmet sınıfa eklemek `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4acb2-345">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="4acb2-346">Veri Hizmeti sınıfındaki kullan:</span><span class="sxs-lookup"><span data-stu-id="4acb2-346">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="4acb2-347">Sık karşılaşılan hatalar</span><span class="sxs-lookup"><span data-stu-id="4acb2-347">Common errors</span></span>

* <span data-ttu-id="4acb2-348">"Hizmet türü 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' için 'Microsoft.AspNetCore.Session.DistributedSessionStore' etkinleştirmeye çalışılırken çözmek alınamıyor."</span><span class="sxs-lookup"><span data-stu-id="4acb2-348">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="4acb2-349">Bu en az bir yapılandırma devrederek genellikle kaynaklanır `IDistributedCache` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="4acb2-349">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="4acb2-350">Daha fazla bilgi için bkz. <xref:performance/caching/distributed> ve <xref:performance/caching/memory>.</span><span class="sxs-lookup"><span data-stu-id="4acb2-350">For more information, see <xref:performance/caching/distributed> and <xref:performance/caching/memory>.</span></span>

* <span data-ttu-id="4acb2-351">Ara yazılım başarısız oturum (örneğin, yedekleme deposunun kullanılabilir durumda değilse) bir oturumu kalıcı olayda ara yazılım özel durumu günlüğe kaydeder ve isteğin normal şekilde devam eder.</span><span class="sxs-lookup"><span data-stu-id="4acb2-351">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="4acb2-352">Bu beklenmeyen davranışa yol açar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-352">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="4acb2-353">Örneğin, bir kullanıcı oturumunda bir alışveriş sepeti depolar.</span><span class="sxs-lookup"><span data-stu-id="4acb2-353">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="4acb2-354">Kullanıcı bir öğeyi sepete ekler, ancak yürütme başarısız.</span><span class="sxs-lookup"><span data-stu-id="4acb2-354">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="4acb2-355">Öğesi true değilse, sepetine eklendi kullanıcıya raporların uygulama başarısızlığı hakkında bilmez.</span><span class="sxs-lookup"><span data-stu-id="4acb2-355">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="4acb2-356">Hataları denetlemek için önerilen yaklaşım çağırmaktır `await feature.Session.CommitAsync();` uygulama bittiğinde uygulama kodundan oturuma yazma.</span><span class="sxs-lookup"><span data-stu-id="4acb2-356">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="4acb2-357">`CommitAsync` yedekleme deposu kullanılamıyorsa bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4acb2-357">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="4acb2-358">Varsa `CommitAsync` başarısız olursa, uygulama özel durumu işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4acb2-358">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="4acb2-359">`LoadAsync` veri deposu kullanılamaz olduğu aynı koşullarda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4acb2-359">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4acb2-360">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4acb2-360">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
