---
title: ASP.NET Core oturum ve uygulama durumu
author: rick-anderson
description: İstekler arasında oturum ve uygulama durumunu korumak için yaklaşım bulur.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 9c63d9313acb055e6c692a7fef3d28e94cb37093
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272889"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="5e5f8-103">ASP.NET Core oturum ve uygulama durumu</span><span class="sxs-lookup"><span data-stu-id="5e5f8-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="5e5f8-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5e5f8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5e5f8-105">HTTP durum bilgisi olmayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="5e5f8-106">Ek adımlar bırakmadan HTTP isteklerini kullanıcı değerleri veya uygulama durumu korumaz bağımsız iletileri edilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="5e5f8-107">Bu makalede istekler arasında kullanıcı verilerini ve uygulama durumunu korumak için çeşitli yaklaşımlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="5e5f8-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e5f8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="5e5f8-109">Durum Yönetimi</span><span class="sxs-lookup"><span data-stu-id="5e5f8-109">State management</span></span>

<span data-ttu-id="5e5f8-110">Durum, çeşitli yaklaşımlar kullanılarak depolanabilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-110">State can be stored using several approaches.</span></span> <span data-ttu-id="5e5f8-111">Her iki yaklaşımın bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="5e5f8-112">Depolama yaklaşımı</span><span class="sxs-lookup"><span data-stu-id="5e5f8-112">Storage approach</span></span> | <span data-ttu-id="5e5f8-113">Depolama mekanizması</span><span class="sxs-lookup"><span data-stu-id="5e5f8-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="5e5f8-114">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="5e5f8-114">Cookies</span></span>](#cookies) | <span data-ttu-id="5e5f8-115">HTTP tanımlama bilgilerini (sunucu tarafı uygulama kodu kullanarak depolanan veriler dahil)</span><span class="sxs-lookup"><span data-stu-id="5e5f8-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="5e5f8-116">oturum durumu</span><span class="sxs-lookup"><span data-stu-id="5e5f8-116">Session state</span></span>](#session-state) | <span data-ttu-id="5e5f8-117">HTTP tanımlama bilgilerini ve sunucu tarafı uygulama kodu</span><span class="sxs-lookup"><span data-stu-id="5e5f8-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="5e5f8-118">TempData</span><span class="sxs-lookup"><span data-stu-id="5e5f8-118">TempData</span></span>](#tempdata) | <span data-ttu-id="5e5f8-119">HTTP tanımlama bilgilerini veya oturum durumu</span><span class="sxs-lookup"><span data-stu-id="5e5f8-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="5e5f8-120">Sorgu dizeleri</span><span class="sxs-lookup"><span data-stu-id="5e5f8-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="5e5f8-121">HTTP sorgu dizeleri</span><span class="sxs-lookup"><span data-stu-id="5e5f8-121">HTTP query strings</span></span> |
| [<span data-ttu-id="5e5f8-122">Gizli alanlar</span><span class="sxs-lookup"><span data-stu-id="5e5f8-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="5e5f8-123">HTTP form alanları</span><span class="sxs-lookup"><span data-stu-id="5e5f8-123">HTTP form fields</span></span> |
| [<span data-ttu-id="5e5f8-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="5e5f8-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="5e5f8-125">Sunucu tarafı uygulama kodu</span><span class="sxs-lookup"><span data-stu-id="5e5f8-125">Server-side app code</span></span> |
| [<span data-ttu-id="5e5f8-126">Önbellek</span><span class="sxs-lookup"><span data-stu-id="5e5f8-126">Cache</span></span>](#cache) | <span data-ttu-id="5e5f8-127">Sunucu tarafı uygulama kodu</span><span class="sxs-lookup"><span data-stu-id="5e5f8-127">Server-side app code</span></span> |
| [<span data-ttu-id="5e5f8-128">Bağımlılık Ekleme</span><span class="sxs-lookup"><span data-stu-id="5e5f8-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="5e5f8-129">Sunucu tarafı uygulama kodu</span><span class="sxs-lookup"><span data-stu-id="5e5f8-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="5e5f8-130">Tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="5e5f8-130">Cookies</span></span>

<span data-ttu-id="5e5f8-131">Tanımlama bilgilerini istekler genelinde verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-131">Cookies store data across requests.</span></span> <span data-ttu-id="5e5f8-132">Tanımlama bilgilerini içeren tüm istekleri gönderildiğinden, kendi boyutu en az olarak tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="5e5f8-133">İdeal olarak, yalnızca bir tanımlayıcı bir tanımlama bilgisinde uygulama tarafından depolanan verilerle depolanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="5e5f8-134">Çoğu tarayıcı tanımlama bilgisi boyutu 4096 bayt kısıtlayın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="5e5f8-135">Yalnızca sınırlı sayıda tanımlama bilgileri, her etki alanı için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="5e5f8-136">Tanımlama bilgilerini oynama tabi olduğundan, uygulama tarafından doğrulanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="5e5f8-137">Tanımlama bilgileri, kullanıcılar tarafından silinebilir ve istemcilerde süresi dolacak.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="5e5f8-138">Ancak, tanımlama bilgileri genellikle en sağlam istemci üzerindeki veri kalıcılığını biçimidir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="5e5f8-139">Tanımlama bilgileri, genellikle içeriği bilinen bir kullanıcı için burada özelleştirilmiş kişiselleştirme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="5e5f8-140">Kullanıcı yalnızca tanımlanan ve çoğu durumda kimliği değil.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="5e5f8-141">Tanımlama bilgisi, kullanıcının adı, hesap adı veya benzersiz bir kullanıcı kimliği (örneğin, bir GUID) depolayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="5e5f8-142">Tanımlama bilgisinin sonra kendi tercih edilen Web sitesi arka plan rengi gibi kullanıcının kişiselleştirilmiş ayarlarına erişmek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="5e5f8-143">Dikkatli olmanızı [Avrupa Birliği genel veri koruma düzenlemeleri (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) zaman tanımlama bilgilerini vermek ve gizlilikle ilgili ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="5e5f8-144">Daha fazla bilgi için bkz: [ASP.NET Core genel veri koruma düzenleme (GDPR) desteği](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="5e5f8-145">oturum durumu</span><span class="sxs-lookup"><span data-stu-id="5e5f8-145">Session state</span></span>

<span data-ttu-id="5e5f8-146">Kullanıcının bir web uygulaması gözatar sırada oturum durumu ASP.NET Core kullanıcı verilerinin depolanması için bir senaryodur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="5e5f8-147">Oturum durumu, istemciden gelen isteklere arasında veri kalıcı hale getirmek için uygulama tarafından korunan bir depolama kullanır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="5e5f8-148">Oturumunun veri önbelleği tarafından yedeklenen ve kısa ömürlü veriler kabul&mdash;site oturum verilerini çalışmaya devam etmelidir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5f8-149">Oturum desteklenmiyor [SignalR](xref:signalr/index) uygulamalar için bir [SignalR hub'ı](xref:signalr/hubs) bağımsız bir HTTP bağlamını yürütür.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-149">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="5e5f8-150">Örneğin, bir uzun zaman oluşabilir yoklama isteği tutulan açık isteğin HTTP bağlamını ömür ötesinde hub tarafından.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-150">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="5e5f8-151">ASP.NET Core her istek ile uygulamaya gönderilen bir oturum kimliği içeren bir istemciye bir tanımlama bilgisi sağlayarak oturum durumunu korur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-151">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="5e5f8-152">Uygulamanın, oturum verilerini getirmek için oturum kimliği kullanır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-152">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="5e5f8-153">Oturum durumu aşağıdaki davranışları sergiler:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-153">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="5e5f8-154">Oturum tanımlama bilgisi tarayıcıya özgü olduğundan, oturumlar tarayıcılar arasında paylaşılmaz.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-154">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="5e5f8-155">Tarayıcı oturumu sona erdiğinde oturum tanımlama bilgileri silinir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-155">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="5e5f8-156">Süresi dolmuş bir oturum için bir tanımlama bilgisi alınmazsa, aynı oturum tanımlama bilgisi kullanan yeni bir oturum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-156">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="5e5f8-157">Boş oturumları olmayan korunur&mdash;oturum içine oturum istekler genelinde kalıcı olması için ayarlama en az bir değere sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-157">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="5e5f8-158">Bir oturumu korunur değil, yeni bir oturum kimliği her yeni bir istek için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-158">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="5e5f8-159">Uygulama, son istekten sonra sınırlı bir süre için bir oturum korur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-159">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="5e5f8-160">Uygulamanın oturum zaman aşımı ayarlar veya 20 dakikalık varsayılan değerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-160">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="5e5f8-161">Oturum durumu, belirli bir oturum için belirli ancak veri oturumlarında kalıcı depolama burada gerektirmeyen kullanıcı verilerini depolamak için idealdir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-161">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="5e5f8-162">Oturum verileri silinir ya da zaman [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) uygulama çağrılmadan veya oturumun süresi dolduğunda.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-162">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="5e5f8-163">Bir istemci tarayıcısı kapatıldı veya ne zaman oturum tanımlama bilgisi silinmiş veya istemcide süresi uygulama kodu bildirmek için varsayılan bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-163">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>

> [!WARNING]
> <span data-ttu-id="5e5f8-164">Hassas verileri kavramak depolamayın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-164">Don't store sensitive data in session state.</span></span> <span data-ttu-id="5e5f8-165">Kullanıcı olmayan Tarayıcıyı kapatın ve oturum tanımlama bilgisi temizleyin.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-165">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="5e5f8-166">Bazı tarayıcılar tarayıcı pencerelerini arasında geçerli bir oturum tanımlama bilgileri korur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-166">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="5e5f8-167">Bir oturum için tek bir kullanıcı kısıtlı olmayabilir&mdash;sonraki kullanıcının aynı oturum tanımlama bilgisiyle uygulama göz atmak devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-167">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="5e5f8-168">Bellek içi önbellek sağlayıcı uygulama bulunduğu sunucu belleğini oturum verilerini depolar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-168">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="5e5f8-169">Bir sunucu grubu senaryoda:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-169">In a server farm scenario:</span></span>

* <span data-ttu-id="5e5f8-170">Kullanım *Yapışkan oturumları* her oturum belirli uygulama örneği için tek bir sunucu üzerinde bağlamanın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-170">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="5e5f8-171">[Azure uygulama hizmeti](https://azure.microsoft.com/services/app-service/) kullanan [uygulama isteği yönlendirme (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) Yapışkan oturumlar varsayılan olarak zorlamak için.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="5e5f8-172">Ancak, Yapışkan oturumları ölçeklenebilirliği etkileyen ve web uygulama güncelleştirmeleri zorlaştırabilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-172">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="5e5f8-173">Bir Redis veya SQL Server kullanmak için daha iyi bir yaklaşım olduğu dağıtılmış Yapışkan oturumları gerektirmeyen önbellek.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-173">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="5e5f8-174">Daha fazla bilgi için bkz: [iş dağıtılmış önbellek ile](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-174">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="5e5f8-175">Oturum tanımlama bilgisi aracılığıyla şifrelenmiş [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-175">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="5e5f8-176">Veri koruma, her makinede oturum tanımlama bilgileri okumak için düzgün şekilde yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-176">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="5e5f8-177">Daha fazla bilgi için bkz: [ASP.NET Çekirdeği'nde veri koruma](xref:security/data-protection/index) ve [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-177">For more information, see [Data Protection in ASP.NET Core](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="5e5f8-178">Oturum durumunu yapılandırın</span><span class="sxs-lookup"><span data-stu-id="5e5f8-178">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-179">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) yer aldığı paket [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), oturum durumunu yönetmek için ara yazılım sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-179">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="5e5f8-180">Oturum Ara etkinleştirmek için `Startup` içermelidir:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-180">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-181">[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) paket oturum durumu yönetmek için ara yazılım sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-181">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="5e5f8-182">Oturum Ara etkinleştirmek için `Startup` içermelidir:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-182">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="5e5f8-183">Herhangi bir [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) bellek önbellekleri.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-183">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="5e5f8-184">`IDistributedCache` Uygulama oturumu için bir yedekleme deposu olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-184">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="5e5f8-185">Daha fazla bilgi için bkz: [iş dağıtılmış önbellek ile](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-185">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>
* <span data-ttu-id="5e5f8-186">Çağrı [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) içinde `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-186">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="5e5f8-187">Çağrı [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) içinde `Configure`.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-187">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="5e5f8-188">Aşağıdaki kod, varsayılan bir bellek içi uygulama bellek içi oturum sağlayıcısı ayarlanacağı gösterilmiştir `IDistributedCache`:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-188">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-189">[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-189">[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-190">[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-190">[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]</span></span>

::: moniker-end

<span data-ttu-id="5e5f8-191">Ara yazılım sıralama önemlidir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-191">The order of middleware is important.</span></span> <span data-ttu-id="5e5f8-192">Önceki örnekte, bir `InvalidOperationException` özel durum oluştu, `UseSession` sonra çağrılan `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-192">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="5e5f8-193">Daha fazla bilgi için bkz: [ara yazılım sıralama](xref:fundamentals/middleware/index#ordering).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-193">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#ordering).</span></span>

<span data-ttu-id="5e5f8-194">[İçeriğin HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) oturum durumu yapılandırıldıktan sonra kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-194">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="5e5f8-195">`HttpContext.Session` önce erişilemez `UseSession` çağrıldı.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-195">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="5e5f8-196">Uygulama yanıt akışına yazmak başladıktan sonra yeni bir oturum tanımlama ile yeni bir oturum oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-196">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="5e5f8-197">Özel durum web sunucusu günlüğüne kaydedilir ve tarayıcıda görüntülenen değil.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-197">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="5e5f8-198">Oturum durumu zaman uyumsuz olarak yükleme</span><span class="sxs-lookup"><span data-stu-id="5e5f8-198">Load session state asynchronously</span></span>

<span data-ttu-id="5e5f8-199">Arka plandaki gelen oturum kayıtları ASP.NET Core varsayılan oturum sağlayıcısında yükler [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) yedekleme deposu zaman uyumsuz olarak yalnızca IF [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) yöntemi açıkça çağrılan önce [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [ayarlamak](/dotnet/api/microsoft.aspnetcore.http.isession.set), veya [kaldırmak](/dotnet/api/microsoft.aspnetcore.http.isession.remove) yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-199">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="5e5f8-200">Varsa `LoadAsync` ilk olarak, temel olarak adlandırılmaz oturum kayıt yüklendiği zaman uyumlu olarak, hangi ölçekte performans cezasına sebep.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-200">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="5e5f8-201">Uygulamaların bu deseni zorlamak için Kaydır [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) ve [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) uygulamaları, bir özel durum sürümleri ile `LoadAsync` değil yöntemi çağrılmadan önce `TryGetValue`, `Set`, veya `Remove`.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-201">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="5e5f8-202">Sarmalanan sürümleri hizmetler kapsayıcısının kaydedin.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-202">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="5e5f8-203">Oturum seçenekleri</span><span class="sxs-lookup"><span data-stu-id="5e5f8-203">Session options</span></span>

<span data-ttu-id="5e5f8-204">Oturum Varsayılanları geçersiz kılmak için kullanın [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-204">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="5e5f8-205">Seçenek</span><span class="sxs-lookup"><span data-stu-id="5e5f8-205">Option</span></span> | <span data-ttu-id="5e5f8-206">Açıklama</span><span class="sxs-lookup"><span data-stu-id="5e5f8-206">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="5e5f8-207">Tanımlama bilgisi</span><span class="sxs-lookup"><span data-stu-id="5e5f8-207">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="5e5f8-208">Tanımlama bilgisi oluşturmak için kullanılan ayarları belirler.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-208">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="5e5f8-209">[Ad](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) varsayılan olarak [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-209">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="5e5f8-210">[Yol](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) varsayılan olarak [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-210">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="5e5f8-211">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) varsayılan olarak [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-211">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="5e5f8-212">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) varsayılan olarak `true`.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-212">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="5e5f8-213">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) varsayılan olarak `false`.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-213">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="5e5f8-214">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="5e5f8-214">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="5e5f8-215">`IdleTimeout` İçeriğinin terk önce ne kadar süreyle oturum boşta kalabileceği gösterir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-215">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="5e5f8-216">Her oturum erişimini zaman aşımı sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-216">Each session access resets the timeout.</span></span> <span data-ttu-id="5e5f8-217">Bu, yalnızca oturum olmayan tanımlama bilgisinin içeriğini geçerlidir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-217">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="5e5f8-218">Varsayılan değer 20 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-218">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="5e5f8-219">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="5e5f8-219">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="5e5f8-220">En yüksek süreyi Mağaza'dan oturum yüklemeye veya geri deposuna kaydetmek için izin.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-220">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="5e5f8-221">Bu yalnızca zaman uyumsuz işlemleri için geçerli unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-221">Note this may only apply to asynchronous operations.</span></span> <span data-ttu-id="5e5f8-222">Bu zaman aşımı kullanarak devre dışı bırakılabilir [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-222">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="5e5f8-223">Varsayılan değer 1 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-223">The default is 1 minute.</span></span> |

<span data-ttu-id="5e5f8-224">Oturum tanımlama bilgisi izlemek ve tek bir tarayıcıdan istekleri belirlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-224">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="5e5f8-225">Varsayılan olarak, bu tanımlama bilgisi adlı `.AspNetCore.Session`, ve bir yol kullanır `/`.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-225">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="5e5f8-226">Tanımlama bilgisi varsayılan etki alanı belirtmiyor olduğundan, istemci tarafı komut dosyası için kullanılabilir sayfada yapılan değil (çünkü [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) varsayılan olarak `true`).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-226">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="5e5f8-227">Seçenek</span><span class="sxs-lookup"><span data-stu-id="5e5f8-227">Option</span></span> | <span data-ttu-id="5e5f8-228">Açıklama</span><span class="sxs-lookup"><span data-stu-id="5e5f8-228">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="5e5f8-229">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="5e5f8-229">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="5e5f8-230">Tanımlama bilgisi oluşturmak için kullanılan etki alanını belirler.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-230">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="5e5f8-231">`CookieDomain` Varsayılan olarak ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-231">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="5e5f8-232">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="5e5f8-232">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="5e5f8-233">Tarayıcı tanımlama bilgisine istemci tarafı JavaScript tarafından erişilecek izin verip vermeyeceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-233">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="5e5f8-234">Varsayılan değer `true`, tanımlama bilgisinin başka bir deyişle, yalnızca HTTP isteklerine geçirilir ve sayfada komut dosyası için kullanılabilir hale değil.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-234">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="5e5f8-235">CookieName</span><span class="sxs-lookup"><span data-stu-id="5e5f8-235">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="5e5f8-236">Oturum kimliği sürdürmek için kullanılan tanımlama bilgisinin adını belirler</span><span class="sxs-lookup"><span data-stu-id="5e5f8-236">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="5e5f8-237">Varsayılan değer [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-237">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="5e5f8-238">CookiePath</span><span class="sxs-lookup"><span data-stu-id="5e5f8-238">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="5e5f8-239">Tanımlama bilgisi oluşturmak için kullanılan yolu belirler.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-239">Determines the path used to create the cookie.</span></span> <span data-ttu-id="5e5f8-240">Varsayılan olarak [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-240">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="5e5f8-241">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="5e5f8-241">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="5e5f8-242">Tanımlama bilgisinin HTTPS isteklerinde yalnızca iletilip iletilmeyeceğini belirler.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-242">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="5e5f8-243">Varsayılan değer [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-243">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="5e5f8-244">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="5e5f8-244">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="5e5f8-245">`IdleTimeout` İçeriğinin terk önce ne kadar süreyle oturum boşta kalabileceği gösterir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-245">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="5e5f8-246">Her oturum erişimini zaman aşımı sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-246">Each session access resets the timeout.</span></span> <span data-ttu-id="5e5f8-247">Bu, yalnızca oturum olmayan tanımlama bilgisinin içeriğini geçerlidir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-247">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="5e5f8-248">Varsayılan değer 20 dakikadır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-248">The default is 20 minutes.</span></span> |

<span data-ttu-id="5e5f8-249">Oturum tanımlama bilgisi izlemek ve tek bir tarayıcıdan istekleri belirlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-249">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="5e5f8-250">Varsayılan olarak, bu tanımlama bilgisi adlı `.AspNet.Session`, ve bir yol kullanır `/`.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-250">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="5e5f8-251">Tanımlama bilgisi oturum Varsayılanları geçersiz kılmak için kullanın `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-251">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-252">[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-252">[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-253">[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-253">[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]</span></span>

::: moniker-end

<span data-ttu-id="5e5f8-254">Uygulama kullandığı [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) sunucusunun önbelleğine içeriğini terk önce ne kadar oturum boşta kalabileceği belirlemek için özellik.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-254">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="5e5f8-255">Bu özellik tanımlama bilgisinin süre sonu bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-255">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="5e5f8-256">Geçtiği her isteği [oturum Ara](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) zaman aşımı sıfırlar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-256">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="5e5f8-257">Oturum durumu *kilitleme*.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-257">Session state is *non-locking*.</span></span> <span data-ttu-id="5e5f8-258">Son istekten iki isteği aynı anda bir oturum içeriğini değiştirmeye çalışırsanız, ilk geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-258">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="5e5f8-259">`Session` olarak uygulanan bir *tutarlı oturum*, yani tüm içeriği birlikte depolanır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-259">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="5e5f8-260">İki isteği farklı oturum değerlerini değiştirmek aradığınızda, son isteği tarafından ilk oturum değişikliklerinin geçersiz kılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-260">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="5e5f8-261">Ayarlama ve oturum değerleri alma</span><span class="sxs-lookup"><span data-stu-id="5e5f8-261">Set and get Session values</span></span>

<span data-ttu-id="5e5f8-262">Oturum durumu Razor sayfalarından erişilen [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) sınıf veya MVC [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller) ile sınıf [içeriğin HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-262">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="5e5f8-263">Bu özellik bir [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) uygulaması.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-263">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-264">`ISession` Uygulamasını tamsayı ve dize değerlerini ayarlama ve alma için birkaç genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-264">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="5e5f8-265">Uzantı yöntemleri [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) ad alanı (eklemek bir `using Microsoft.AspNetCore.Http;` genişletme yöntemleri erişmek için deyimi) olduğunda [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) Paket proje tarafından başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-265">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="5e5f8-266">Her iki paketin içinde yer alan [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-266">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-267">`ISession` Uygulamasını tamsayı ve dize değerlerini ayarlama ve alma için birkaç genişletme yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-267">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="5e5f8-268">Uzantı yöntemleri [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) ad alanı (eklemek bir `using Microsoft.AspNetCore.Http;` genişletme yöntemleri erişmek için deyimi) olduğunda [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) Paket proje tarafından başvuruluyor.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-268">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="5e5f8-269">`ISession` genişletme yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-269">`ISession` extension methods:</span></span>

* [<span data-ttu-id="5e5f8-270">Get (ISession, dize)</span><span class="sxs-lookup"><span data-stu-id="5e5f8-270">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="5e5f8-271">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="5e5f8-271">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="5e5f8-272">GetString (ISession, dize)</span><span class="sxs-lookup"><span data-stu-id="5e5f8-272">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="5e5f8-273">Setınt32 (ISession, dize, Int32)</span><span class="sxs-lookup"><span data-stu-id="5e5f8-273">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="5e5f8-274">SetString (ISession, dize, dize)</span><span class="sxs-lookup"><span data-stu-id="5e5f8-274">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="5e5f8-275">Aşağıdaki örnekte oturum değerini alır. `IndexModel.SessionKeyName` anahtarı (`_Name` örnek uygulamasında) bir Razor sayfalarının sayfasında:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-275">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="5e5f8-276">Aşağıdaki örnek, ayarlama ve bir tamsayı ve bir dize alma gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-276">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-277">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-277">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-278">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-278">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]</span></span>

::: moniker-end

<span data-ttu-id="5e5f8-279">Bellek içi önbellek kullanırken bile bir dağıtılmış önbellek senaryosunu sağlamak üzere tüm oturum verilerini seri hale getirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-279">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="5e5f8-280">En az dize ve sayı serileştiricileri verilmiştir (ve uzantı yöntemleri, bkz: [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-280">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="5e5f8-281">JSON gibi başka bir mekanizma kullanarak kullanıcı tarafından karmaşık türler seri hale getirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-281">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="5e5f8-282">Ayarlama ve serileştirilebilir nesneler alma için aşağıdaki genişletme yöntemleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-282">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-283">[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-283">[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-284">[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-284">[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]</span></span>

::: moniker-end

<span data-ttu-id="5e5f8-285">Aşağıdaki örnek, ayarlama ve serileştirilebilir bir nesne ile genişletme yöntemleri alma gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-285">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-286">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-286">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-287">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-287">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]</span></span>

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="5e5f8-288">TempData</span><span class="sxs-lookup"><span data-stu-id="5e5f8-288">TempData</span></span>

<span data-ttu-id="5e5f8-289">ASP.NET Core sunan [Razor sayfalarının sayfa modelin TempData özelliği](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) veya [MVC denetleyicisi TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-289">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="5e5f8-290">Bu özellik, dosyayı okuma kadar verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-290">This property stores data until it's read.</span></span> <span data-ttu-id="5e5f8-291">[Tutmak](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) ve [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) yöntemleri, verileri silme olmadan incelemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-291">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="5e5f8-292">Veriler birden çok tek bir istek için gerekli olduğunda TempData yeniden yönlendirme için özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-292">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="5e5f8-293">TempData tanımlama bilgilerini veya oturum durumu kullanarak TempData sağlayıcıları tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-293">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="5e5f8-294">TempData sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="5e5f8-294">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-295">ASP.NET Core 2.0 veya sonraki sürümlerde, tanımlama bilgisi tabanlı TempData sağlayıcısı TempData tanımlama bilgilerini depolamak için varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-295">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="5e5f8-296">Tanımlama bilgisi verileri kullanılarak şifrelenir [Idataprotector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), ile kodlanmış [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), ardından öbekli.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-296">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="5e5f8-297">Tanımlama bilgisinin öbekli çünkü tek tanımlama bilgisi ASP.NET 1.x uygulanmaz çekirdek bulunan sınır boyutu.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-297">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="5e5f8-298">Şifrelenmiş verileri sıkıştırmak için güvenlik sorunları gibi yol açabileceğinden tanımlama bilgisi verileri sıkıştırılmaz [SUÇ](https://wikipedia.org/wiki/CRIME_(security_exploit)) ve [ihlali](https://wikipedia.org/wiki/BREACH_(security_exploit)) saldırıları.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-298">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="5e5f8-299">Tanımlama bilgisi tabanlı TempData sağlayıcısı hakkında daha fazla bilgi için bkz: [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-299">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-300">ASP.NET Core 1.0 ve 1.1, oturum durumu TempData sağlayıcısı varsayılan sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-300">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="5e5f8-301">TempData bir sağlayıcı seçin</span><span class="sxs-lookup"><span data-stu-id="5e5f8-301">Choose a TempData provider</span></span>

<span data-ttu-id="5e5f8-302">TempData sağlayıcısı seçme bazı noktalar gibi içerir:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-302">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="5e5f8-303">Uygulama zaten oturum durumu kullanıyor mu?</span><span class="sxs-lookup"><span data-stu-id="5e5f8-303">Does the app already use session state?</span></span> <span data-ttu-id="5e5f8-304">Bu durumda, oturum durumu TempData sağlayıcısı kullanarak uygulama (yanı sıra veri boyutu) için ek ücret ödemeden sahiptir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-304">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="5e5f8-305">Uygulama TempData yalnızca tutumlu kullanmaz görece küçük miktarda veriyi (en fazla 500 bayt) için?</span><span class="sxs-lookup"><span data-stu-id="5e5f8-305">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="5e5f8-306">Böylece, tanımlama bilgisi TempData sağlayıcı her istek için küçük bir maliyet eklerse, TempData taşır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-306">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="5e5f8-307">Aksi durumda, oturum durumu TempData sağlayıcısı TempData tüketilen kadar gidiş büyük miktarda veri her istekte önlemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-307">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="5e5f8-308">Uygulamayı bir sunucu grubunda birden çok sunucu üzerinde çalışıyor mu?</span><span class="sxs-lookup"><span data-stu-id="5e5f8-308">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="5e5f8-309">Bu nedenle, varsa tanımlama bilgisi TempData sağlayıcısı veri koruması dışında kullanmak için gereken ek yapılandırma (bkz [veri koruması](xref:security/data-protection/index) ve [anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers)).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-309">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see [Data Protection](xref:security/data-protection/index) and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="5e5f8-310">Çoğu web istemcileri (örneğin, web tarayıcıları) her tanımlama bilgisi, tanımlama bilgilerinin toplam sayısı veya her ikisi de en büyük boyutu sınırları uygulayın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-310">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="5e5f8-311">Tanımlama bilgisi TempData sağlayıcı kullanırken, uygulama bu sınırı aşan olmaz doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-311">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="5e5f8-312">Verilerin toplam boyutu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-312">Consider the total size of the data.</span></span> <span data-ttu-id="5e5f8-313">Hesap için şifreleme ve Öbekleme nedeniyle tanımlama bilgisi boyutu artar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-313">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="5e5f8-314">TempData sağlayıcısı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="5e5f8-314">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-315">Tanımlama bilgisi tabanlı TempData sağlayıcısı varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-315">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="5e5f8-316">Oturum tabanlı TempData sağlayıcı etkinleştirmek için [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-316">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

<span data-ttu-id="5e5f8-317">[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-317">[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-318">Aşağıdaki `Startup` sınıf kodu oturum tabanlı TempData sağlayıcı yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-318">The following `Startup` class code configures the session-based TempData provider:</span></span>

<span data-ttu-id="5e5f8-319">[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-319">[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]</span></span>

::: moniker-end

<span data-ttu-id="5e5f8-320">Ara yazılım sıralama önemlidir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-320">The order of middleware is important.</span></span> <span data-ttu-id="5e5f8-321">Önceki örnekte, bir `InvalidOperationException` özel durum oluştu, `UseSession` sonra çağrılan `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-321">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="5e5f8-322">Daha fazla bilgi için bkz: [ara yazılım sıralama](xref:fundamentals/middleware/index#ordering).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-322">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#ordering).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5e5f8-323">.NET Framework hedefleme ve oturum tabanlı TempData sağlayıcısını kullanarak eklerseniz, [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) projeye paket.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-323">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="5e5f8-324">Sorgu dizeleri</span><span class="sxs-lookup"><span data-stu-id="5e5f8-324">Query strings</span></span>

<span data-ttu-id="5e5f8-325">Verileri sınırlı miktarda bir istekten başka bir yeni isteğin sorgu dizesi için ekleyerek geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-325">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="5e5f8-326">Bu durum e-posta veya sosyal ağlar paylaşılan katıştırılmış durumuyla bağlantılarına izin verir kalıcı bir şekilde yakalamak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-326">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="5e5f8-327">Hiçbir zaman URL sorgu dizeleri ortak olduğundan, sorgu dizeleri için hassas verileri kullanın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-327">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="5e5f8-328">İstenmeyen paylaşımı ek olarak, sorgu dizelerine verileri dahil olmak üzere fırsatı oluşturabilirsiniz [siteler arası istek sahteciliği (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) kullanıcıların kimlik doğrulaması sırasında kötü amaçlı siteleri ziyaret içine kandırarak saldırıları.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-328">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="5e5f8-329">Saldırganlar uygulamasından kullanıcı verileri çalmaya veya kullanıcı adına kötü amaçlı eylemleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-329">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="5e5f8-330">Herhangi bir korunan uygulama veya oturum durumu CSRF saldırılarına karşı korumanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-330">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="5e5f8-331">Daha fazla bilgi için bkz: [önlemek siteler arası istek sahtekarlığı (XSRF/CSRF) saldırılarını](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-331">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="5e5f8-332">Gizli alanlar</span><span class="sxs-lookup"><span data-stu-id="5e5f8-332">Hidden fields</span></span>

<span data-ttu-id="5e5f8-333">Veri gizli form alanlarını kaydedilir ve bir sonraki istekte geri gönderilen.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-333">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="5e5f8-334">Bu, çok sayfalı formlarında yaygındır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-334">This is common in multi-page forms.</span></span> <span data-ttu-id="5e5f8-335">İstemci olası verileri değiştirme olduğundan, uygulama her zaman gizli alanlarında depolanan verilerin düzeltin gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-335">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="5e5f8-336">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="5e5f8-336">HttpContext.Items</span></span>

<span data-ttu-id="5e5f8-337">[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) koleksiyonu, tek bir isteği işlerken verileri depolamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-337">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="5e5f8-338">Bir istek işlendikten sonra koleksiyonun içeriğini atılır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-338">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="5e5f8-339">`Items` Koleksiyonu genellikle bileşenler veya farklı zamanlarda isteği sırasında zaman çalışır ve parametreleri geçirmek için hiçbir doğrudan şekilde iletişim kurmak için ara yazılım izin vermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-339">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="5e5f8-340">Aşağıdaki örnekte, [ara yazılımı](xref:fundamentals/middleware/index) ekler `isVerified` için `Items` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-340">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="5e5f8-341">Değerini başka bir ara yazılım ardışık erişebilirsiniz `isVerified`:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-341">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="5e5f8-342">Yalnızca tek bir uygulama tarafından kullanılan ara yazılımı `string` anahtarları kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-342">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="5e5f8-343">Uygulama örnekleri arasında paylaşılan Ara benzersiz nesne anahtarları anahtar çakışmaları önlemek için kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-343">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="5e5f8-344">Aşağıdaki örnek, bir ara yazılım sınıfında tanımlanan benzersiz nesne anahtarı kullanmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-344">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-345">[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-345">[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-346">[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-346">[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]</span></span>

::: moniker-end

<span data-ttu-id="5e5f8-347">Başka bir kod depolanan değerine erişebilirsiniz `HttpContext.Items` ara yazılım sınıfı tarafından kullanıma sunulan anahtarı kullanarak:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-347">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5e5f8-348">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-348">[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5e5f8-349">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="5e5f8-349">[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]</span></span>

::: moniker-end

<span data-ttu-id="5e5f8-350">Bu yaklaşım da koddaki anahtar dizelerinin kullanılmasını ortadan avantajı vardır.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-350">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="5e5f8-351">Önbellek</span><span class="sxs-lookup"><span data-stu-id="5e5f8-351">Cache</span></span>

<span data-ttu-id="5e5f8-352">Önbelleğe alma, depolamak ve veri almak için etkili bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-352">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="5e5f8-353">Uygulama, önbelleğe alınmış öğeleri ömrü kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-353">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="5e5f8-354">Önbelleğe alınmış verileri belirli bir istek, kullanıcı veya oturum ile ilişkili değil.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-354">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="5e5f8-355">**Dikkatli olun, diğer kullanıcıların isteklerine tarafından alınan önbellek kullanıcıya özgü verilere değil.**</span><span class="sxs-lookup"><span data-stu-id="5e5f8-355">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="5e5f8-356">Daha fazla bilgi için bkz: [yanıtlarını önbelleğe](xref:performance/caching/index) konu.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-356">For more information, see the [Cache responses](xref:performance/caching/index) topic.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="5e5f8-357">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="5e5f8-357">Dependency Injection</span></span>

<span data-ttu-id="5e5f8-358">Kullanım [bağımlılık ekleme](xref:fundamentals/dependency-injection) veri tüm kullanıcılar tarafından kullanılabilmesini sağlamak için:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-358">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="5e5f8-359">Veri içeren bir hizmet tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-359">Define a service containing the data.</span></span> <span data-ttu-id="5e5f8-360">Örneğin, adlı bir sınıf `MyAppData` tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-360">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="5e5f8-361">Hizmet sınıfına ekleyin `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-361">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="5e5f8-362">Veri Hizmeti sınıfı kullanılmasına neden:</span><span class="sxs-lookup"><span data-stu-id="5e5f8-362">Consume the data service class:</span></span>

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

## <a name="common-errors"></a><span data-ttu-id="5e5f8-363">Sık karşılaşılan hataları</span><span class="sxs-lookup"><span data-stu-id="5e5f8-363">Common errors</span></span>

* <span data-ttu-id="5e5f8-364">"Çözümlenemiyor hizmet türü 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' için 'Microsoft.AspNetCore.Session.DistributedSessionStore' etkinleştirmeye çalışırken."</span><span class="sxs-lookup"><span data-stu-id="5e5f8-364">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="5e5f8-365">En az bir yapılandırmak devrederek nedeni genellikle `IDistributedCache` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-365">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="5e5f8-366">Daha fazla bilgi için bkz: [iş ile dağıtılmış önbellek](xref:performance/caching/distributed) ve [bellek içi önbellek](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="5e5f8-366">For more information, see [Work with a distributed cache](xref:performance/caching/distributed) and [Cache in-memory](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="5e5f8-367">Ara yazılım başarısız oturum (örneğin, yedekleme deposu kullanılamıyorsa) oturumu kalıcı olayda ara yazılım özel durumu günlüğe kaydeder ve isteğin normal şekilde devam eder.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-367">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="5e5f8-368">Bu, beklenmeyen davranışa yol açar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-368">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="5e5f8-369">Örneğin, bir kullanıcı oturumu alışveriş sepeti depolar.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-369">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="5e5f8-370">Kullanıcı bir öğeyi sepete ekler, ancak yürütme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-370">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="5e5f8-371">Öğe doğru değilse, sepete eklendi kullanıcıya raporları için uygulama hatası hakkında bilmiyor.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-371">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="5e5f8-372">Hataları denetlemek için önerilen yaklaşım çağırmaktır `await feature.Session.CommitAsync();` uygulama yapıldığında uygulama kodundan oturuma yazma.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-372">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="5e5f8-373">`CommitAsync` yedekleme deposu kullanılamıyorsa, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-373">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="5e5f8-374">Varsa `CommitAsync` başarısız olursa, uygulama özel durumu işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-374">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="5e5f8-375">`LoadAsync` veri deposu kullanılamaz olduğu aynı koşullar altında oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5e5f8-375">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>
