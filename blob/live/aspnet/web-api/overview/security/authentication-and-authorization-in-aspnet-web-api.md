---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: "Kimlik doğrulama ve yetkilendirme ASP.NET Web API'de | Microsoft Docs"
author: MikeWasson
description: "ASP.NET Web API'de kimlik doğrulama ve yetkilendirme genel bir bakış sağlar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 137ac45166be03ae3c4864f41666d2acd1a37dc2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a><span data-ttu-id="a58aa-103">Kimlik doğrulama ve yetkilendirme ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a58aa-103">Authentication and Authorization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="a58aa-104">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a58aa-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a58aa-105">Web API'si oluşturdunuz, ancak şimdi erişimi denetlemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="a58aa-105">You've created a web API, but now you want to control access to it.</span></span> <span data-ttu-id="a58aa-106">Bu makaleler dizide yetkisiz kullanıcılara karşı bir web API güvenliğini sağlamak için bazı seçenekler inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="a58aa-106">In this series of articles, we'll look at some options for securing a web API from unauthorized users.</span></span> <span data-ttu-id="a58aa-107">Bu seri, kimlik doğrulama ve yetkilendirme ele alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="a58aa-107">This series will cover both authentication and authorization.</span></span>

- <span data-ttu-id="a58aa-108">*Kimlik doğrulama* kullanıcının kimliğini bilmektir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-108">*Authentication* is knowing the identity of the user.</span></span> <span data-ttu-id="a58aa-109">Örneğin, Alice kendi kullanıcı adı ve parolayla oturum ve sunucu Alice kimliğini doğrulamak için parolayı kullanır.</span><span class="sxs-lookup"><span data-stu-id="a58aa-109">For example, Alice logs in with her username and password, and the server uses the password to authenticate Alice.</span></span>
- <span data-ttu-id="a58aa-110">*Yetkilendirme* bir kullanıcı bir eylemi gerçekleştirmek için izin verilip verilmediğini karar verme.</span><span class="sxs-lookup"><span data-stu-id="a58aa-110">*Authorization* is deciding whether a user is allowed to perform an action.</span></span> <span data-ttu-id="a58aa-111">Örneğin, bir kaynak alma ancak kaynak oluşturmaz izni Gamze vardır.</span><span class="sxs-lookup"><span data-stu-id="a58aa-111">For example, Alice has permission to get a resource but not create a resource.</span></span>

<span data-ttu-id="a58aa-112">Serideki ilk makale ASP.NET Web API'de kimlik doğrulama ve yetkilendirme genel bir bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="a58aa-112">The first article in the series gives a general overview of authentication and authorization in ASP.NET Web API.</span></span> <span data-ttu-id="a58aa-113">Diğer konular, genel kimlik doğrulama senaryoları için Web API açıklar.</span><span class="sxs-lookup"><span data-stu-id="a58aa-113">Other topics describe common authentication scenarios for Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="a58aa-114">Bu seri gözden geçirdikten ve değerli geri bildirim sağlanan kişilerin sayesinde: Rick Anderson, Levi Broderick, Hakan Dorrans, zel Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.</span><span class="sxs-lookup"><span data-stu-id="a58aa-114">Thanks to the people who reviewed this series and provided valuable feedback: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.</span></span>


## <a name="authentication"></a><span data-ttu-id="a58aa-115">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="a58aa-115">Authentication</span></span>

<span data-ttu-id="a58aa-116">Web API, kimlik doğrulamasının ana bilgisayarda gerçekleşir varsayar.</span><span class="sxs-lookup"><span data-stu-id="a58aa-116">Web API assumes that authentication happens in the host.</span></span> <span data-ttu-id="a58aa-117">Web barındırma için kimlik doğrulaması için HTTP modülleri kullanan IIS bir ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="a58aa-117">For web-hosting, the host is IIS, which uses HTTP modules for authentication.</span></span> <span data-ttu-id="a58aa-118">IIS veya ASP.NET yerleşik olarak kimlik doğrulaması modüllerinden birini kullanmak için projenizin yapılandırabilir veya özel kimlik doğrulama gerçekleştirmek için kendi HTTP modülü yazma.</span><span class="sxs-lookup"><span data-stu-id="a58aa-118">You can configure your project to use any of the authentication modules built in to IIS or ASP.NET, or write your own HTTP module to perform custom authentication.</span></span>

<span data-ttu-id="a58aa-119">Ana kullanıcı kimliği doğruladığında oluşturduğu bir *asıl*, olduğu bir [IPrincipal](https://msdn.microsoft.com/en-us/library/System.Security.Principal.IPrincipal.aspx) kodu altında çalıştığı güvenlik bağlamını temsil eden nesne.</span><span class="sxs-lookup"><span data-stu-id="a58aa-119">When the host authenticates the user, it creates a *principal*, which is an [IPrincipal](https://msdn.microsoft.com/en-us/library/System.Security.Principal.IPrincipal.aspx) object that represents the security context under which code is running.</span></span> <span data-ttu-id="a58aa-120">Ana bilgisayar sorumlu geçerli iş parçacığına ayarlayarak iliştirir **Thread.CurrentPrincipal**.</span><span class="sxs-lookup"><span data-stu-id="a58aa-120">The host attaches the principal to the current thread by setting **Thread.CurrentPrincipal**.</span></span> <span data-ttu-id="a58aa-121">Asıl ilişkili bir içeren **kimlik** kullanıcı hakkındaki bilgileri içeren nesne.</span><span class="sxs-lookup"><span data-stu-id="a58aa-121">The principal contains an associated **Identity** object that contains information about the user.</span></span> <span data-ttu-id="a58aa-122">Kullanıcı kimlik doğrulaması gerekiyorsa, **Identity.IsAuthenticated** özelliği döndürür **doğru**.</span><span class="sxs-lookup"><span data-stu-id="a58aa-122">If the user is authenticated, the **Identity.IsAuthenticated** property returns **true**.</span></span> <span data-ttu-id="a58aa-123">Anonim istekler için **IsAuthenticated** döndürür **false**.</span><span class="sxs-lookup"><span data-stu-id="a58aa-123">For anonymous requests, **IsAuthenticated** returns **false**.</span></span> <span data-ttu-id="a58aa-124">İlkeleri hakkında daha fazla bilgi için bkz: [rol tabanlı güvenlik](https://msdn.microsoft.com/en-us/library/shz8h065.aspx).</span><span class="sxs-lookup"><span data-stu-id="a58aa-124">For more information about principals, see [Role-Based Security](https://msdn.microsoft.com/en-us/library/shz8h065.aspx).</span></span>

### <a name="http-message-handlers-for-authentication"></a><span data-ttu-id="a58aa-125">Kimlik doğrulaması için HTTP ileti işleyicileri</span><span class="sxs-lookup"><span data-stu-id="a58aa-125">HTTP Message Handlers for Authentication</span></span>

<span data-ttu-id="a58aa-126">Ana bilgisayar kimlik doğrulaması için kullanmak yerine, kimlik doğrulaması mantığı içine koyabilirsiniz bir [HTTP ileti işleyicisi](../advanced/http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="a58aa-126">Instead of using the host for authentication, you can put authentication logic into an [HTTP message handler](../advanced/http-message-handlers.md).</span></span> <span data-ttu-id="a58aa-127">Bu durumda, ileti işleyicisi HTTP isteği inceler ve sorumluyu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a58aa-127">In that case, the message handler examines the HTTP request and sets the principal.</span></span>

<span data-ttu-id="a58aa-128">İleti işleyicileri için kimlik doğrulaması kullandığınızda?</span><span class="sxs-lookup"><span data-stu-id="a58aa-128">When should you use message handlers for authentication?</span></span> <span data-ttu-id="a58aa-129">Bazı bileşim şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a58aa-129">Here are some tradeoffs:</span></span>

- <span data-ttu-id="a58aa-130">HTTP modülü ASP.NET kanalı yoluyla Git tüm istekleri görür.</span><span class="sxs-lookup"><span data-stu-id="a58aa-130">An HTTP module sees all requests that go through the ASP.NET pipeline.</span></span> <span data-ttu-id="a58aa-131">Bir ileti işleyicisini yalnızca Web API'sine yönlendirilir istekleri görür.</span><span class="sxs-lookup"><span data-stu-id="a58aa-131">A message handler only sees requests that are routed to Web API.</span></span>
- <span data-ttu-id="a58aa-132">Rota başına ileti işleyicileri olanağı sağlayan bir kimlik doğrulama düzeni özel bir yol uygulamak ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a58aa-132">You can set per-route message handlers, which lets you apply an authentication scheme to a specific route.</span></span>
- <span data-ttu-id="a58aa-133">HTTP modülleri IIS'e özgüdür.</span><span class="sxs-lookup"><span data-stu-id="a58aa-133">HTTP modules are specific to IIS.</span></span> <span data-ttu-id="a58aa-134">İleti işleyicileri konak bağımsız olduğundan, web barındırma hem kendi kendine barındırma ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-134">Message handlers are host-agnostic, so they can be used with both web-hosting and self-hosting.</span></span>
- <span data-ttu-id="a58aa-135">HTTP modülleri IIS günlüğe kaydetme, Denetim ve benzeri katılın.</span><span class="sxs-lookup"><span data-stu-id="a58aa-135">HTTP modules participate in IIS logging, auditing, and so on.</span></span>
- <span data-ttu-id="a58aa-136">HTTP modülleri ardışık düzeninde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a58aa-136">HTTP modules run earlier in the pipeline.</span></span> <span data-ttu-id="a58aa-137">Kimlik doğrulama ileti işleyicisi'ndeki işlemek, işleyici çalıştırılana kadar asıl ayarlamaz.</span><span class="sxs-lookup"><span data-stu-id="a58aa-137">If you handle authentication in a message handler, the principal does not get set until the handler runs.</span></span> <span data-ttu-id="a58aa-138">Ayrıca, ileti işleyicisi yanıt ayrıldığında, asıl önceki asıl geri döner.</span><span class="sxs-lookup"><span data-stu-id="a58aa-138">Moreover, the principal reverts back to the previous principal when the response leaves the message handler.</span></span>

<span data-ttu-id="a58aa-139">Genellikle, bir HTTP modülü kendi kendine barındırma desteği gerekmiyorsa, daha iyi bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-139">Generally, if you don't need to support self-hosting, an HTTP module is a better option.</span></span> <span data-ttu-id="a58aa-140">Kendi kendine barındırma desteklemeniz gerekiyorsa, bir ileti işleyicisini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="a58aa-140">If you need to support self-hosting, consider a message handler.</span></span>

### <a name="setting-the-principal"></a><span data-ttu-id="a58aa-141">Asıl ayarlama</span><span class="sxs-lookup"><span data-stu-id="a58aa-141">Setting the Principal</span></span>

<span data-ttu-id="a58aa-142">Uygulamanız herhangi bir özel kimlik doğrulama mantık gerçekleştirirse, asıl üzerinde iki yerde ayarlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="a58aa-142">If your application performs any custom authentication logic, you must set the principal on two places:</span></span>

- <span data-ttu-id="a58aa-143">**Thread.CurrentPrincipal**.</span><span class="sxs-lookup"><span data-stu-id="a58aa-143">**Thread.CurrentPrincipal**.</span></span> <span data-ttu-id="a58aa-144">Bu özellik, iş parçacığının asıl .NET ayarlamak için standart bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="a58aa-144">This property is the standard way to set the thread's principal in .NET.</span></span>
- <span data-ttu-id="a58aa-145">**HttpContext.Current.User**.</span><span class="sxs-lookup"><span data-stu-id="a58aa-145">**HttpContext.Current.User**.</span></span> <span data-ttu-id="a58aa-146">Bu özellik, ASP.NET ile özeldir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-146">This property is specific to ASP.NET.</span></span>

<span data-ttu-id="a58aa-147">Aşağıdaki kod, asıl ayarlamak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a58aa-147">The following code shows how to set the principal:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="a58aa-148">Web barındırma için her iki yerde de asıl ayarlamanız gerekir; Aksi durumda güvenlik bağlamı tutarsız hale gelebilir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-148">For web-hosting, you must set the principal in both places; otherwise the security context may become inconsistent.</span></span> <span data-ttu-id="a58aa-149">Ancak, kendi kendine barındırma için **HttpContext.Current** null.</span><span class="sxs-lookup"><span data-stu-id="a58aa-149">For self-hosting, however, **HttpContext.Current** is null.</span></span> <span data-ttu-id="a58aa-150">Kodunuz: ana bilgisayar belirsiz emin olmak için bu nedenle, null atamadan önce kontrol **HttpContext.Current**gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="a58aa-150">To ensure your code is host-agnostic, therefore, check for null before assigning to **HttpContext.Current**, as shown.</span></span>

## <a name="authorization"></a><span data-ttu-id="a58aa-151">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="a58aa-151">Authorization</span></span>

<span data-ttu-id="a58aa-152">Yetkilendirme olur ardışık denetleyiciye yakın.</span><span class="sxs-lookup"><span data-stu-id="a58aa-152">Authorization happens later in the pipeline, closer to the controller.</span></span> <span data-ttu-id="a58aa-153">Bu kaynaklara erişim izni olduğunda daha ayrıntılı seçimleri yapmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="a58aa-153">That lets you make more granular choices when you grant access to resources.</span></span>

- <span data-ttu-id="a58aa-154">*Yetkilendirme filtreleri* önce denetleyici eylemini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a58aa-154">*Authorization filters* run before the controller action.</span></span> <span data-ttu-id="a58aa-155">İstek yetkili değil, filtre bir hata yanıtı döndürür ve eylem çağrılamaz.</span><span class="sxs-lookup"><span data-stu-id="a58aa-155">If the request is not authorized, the filter returns an error response, and the action is not invoked.</span></span>
- <span data-ttu-id="a58aa-156">Bir denetleyici eylemi içinde gelen geçerli sorumlu alabilirsiniz **ApiController.User** özelliği.</span><span class="sxs-lookup"><span data-stu-id="a58aa-156">Within a controller action, you can get the current principal from the **ApiController.User** property.</span></span> <span data-ttu-id="a58aa-157">Örneğin, o kullanıcıya ait kaynak döndürerek kullanıcı adına göre kaynakların bir listesini filtreleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a58aa-157">For example, you might filter a list of resources based on the user name, returning only those resources that belong to that user.</span></span>

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a><span data-ttu-id="a58aa-158">Kullanma [yetkilendirmek] özniteliği</span><span class="sxs-lookup"><span data-stu-id="a58aa-158">Using the [Authorize] Attribute</span></span>

<span data-ttu-id="a58aa-159">Web API'si sağlar yerleşik yetkilendirme filtresi [AuthorizeAttribute](https://msdn.microsoft.com/en-us/library/system.web.http.authorizeattribute.aspx).</span><span class="sxs-lookup"><span data-stu-id="a58aa-159">Web API provides a built-in authorization filter, [AuthorizeAttribute](https://msdn.microsoft.com/en-us/library/system.web.http.authorizeattribute.aspx).</span></span> <span data-ttu-id="a58aa-160">Bu filtre, kullanıcının kimliği doğrulanır olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="a58aa-160">This filter checks whether the user is authenticated.</span></span> <span data-ttu-id="a58aa-161">Aksi durumda, eylemi çağırma olmadan HTTP durum kodunu 401 (yetkisiz) döndürür.</span><span class="sxs-lookup"><span data-stu-id="a58aa-161">If not, it returns HTTP status code 401 (Unauthorized), without invoking the action.</span></span>

<span data-ttu-id="a58aa-162">Genel olarak, denetleyici düzeyinde veya inidivual Eylemler düzeyinde filtre uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a58aa-162">You can apply the filter globally, at the controller level, or at the level of inidivual actions.</span></span>

<span data-ttu-id="a58aa-163">**Genel olarak**: tüm Web API denetleyicilerinin erişimini kısıtlamak için ekleme **AuthorizeAttribute** genel filtre listesine filtre:</span><span class="sxs-lookup"><span data-stu-id="a58aa-163">**Globally**: To restrict access for every Web API controller, add the **AuthorizeAttribute** filter to the global filter list:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="a58aa-164">**Denetleyici**: Filtre belirli bir denetleyicinin erişimini kısıtlamak için denetleyici için bir öznitelik olarak Ekle:</span><span class="sxs-lookup"><span data-stu-id="a58aa-164">**Controller**: To restrict access for a specific controller, add the filter as an attribute to the controller:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="a58aa-165">**Eylem**: belirli eylemlerin erişimini kısıtlamak için özniteliği eylem yöntemine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a58aa-165">**Action**: To restrict access for specific actions, add the attribute to the action method:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="a58aa-166">Alternatif olarak, denetleyiciyi kısıtlayıp ve ardından kullanarak belirli eylemlere anonim erişime izin `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a58aa-166">Alternatively, you can restrict the controller and then allow anonymous access to specific actions, by using the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="a58aa-167">Aşağıdaki örnekte, `Post` yöntemi kısıtlıdır, ancak `Get` yöntemi anonim erişime izin verir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-167">In the following example, the `Post` method is restricted, but the `Get` method allows anonymous access.</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="a58aa-168">Önceki örneklerde, filtre tüm kimliği doğrulanmış kullanıcının sınırlı yöntemleri erişmesine izin verir; Yalnızca anonim kullanıcılar tutulur. Ayrıca belirli kullanıcılara veya belirli rollere kullanıcılara erişimi sınırlandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a58aa-168">In the previous examples, the filter allows any authenticated user to access the restricted methods; only anonymous users are kept out. You can also limit access to specific users or to users in specific roles:</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="a58aa-169">**AuthorizeAttribute** Web API denetleyicilerinin filtre bulunduğu **System.Web.Http** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="a58aa-169">The **AuthorizeAttribute** filter for Web API controllers is located in the **System.Web.Http** namespace.</span></span> <span data-ttu-id="a58aa-170">MVC denetleyicileri için benzer bir filtre yok **System.Web.Mvc** Web API denetleyicilerinin ile uyumlu olmayan ad alanı.</span><span class="sxs-lookup"><span data-stu-id="a58aa-170">There is a similar filter for MVC controllers in the **System.Web.Mvc** namespace, which is not compatible with Web API controllers.</span></span>


### <a name="custom-authorization-filters"></a><span data-ttu-id="a58aa-171">Özel yetkilendirme filtreleri</span><span class="sxs-lookup"><span data-stu-id="a58aa-171">Custom Authorization Filters</span></span>

<span data-ttu-id="a58aa-172">Özel yetkilendirme filtresi yazmak için şu türlerden birine türetir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-172">To write a custom authorization filter, derive from one of these types:</span></span>

- <span data-ttu-id="a58aa-173">**AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="a58aa-173">**AuthorizeAttribute**.</span></span> <span data-ttu-id="a58aa-174">Geçerli kullanıcı ve kullanıcı rolleri dayalı yetkilendirme mantığı gerçekleştirmek için bu sınıfı genişletir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-174">Extend this class to perform authorization logic based on the current user and the user's roles.</span></span>
- <span data-ttu-id="a58aa-175">**AuthorizationFilterAttribute**.</span><span class="sxs-lookup"><span data-stu-id="a58aa-175">**AuthorizationFilterAttribute**.</span></span> <span data-ttu-id="a58aa-176">Mutlaka geçerli kullanıcı veya rol dayanmıyor zaman uyumlu yetkilendirme mantığı gerçekleştirmek için bu sınıfı genişletir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-176">Extend this class to perform synchronous authorization logic that is not necessarily based on the current user or role.</span></span>
- <span data-ttu-id="a58aa-177">**IAuthorizationFilter**.</span><span class="sxs-lookup"><span data-stu-id="a58aa-177">**IAuthorizationFilter**.</span></span> <span data-ttu-id="a58aa-178">Zaman uyumsuz yetkilendirme mantığı gerçekleştirmek için bu arabirimi uygulamalıdır; Örneğin, yetkilendirme mantığınızı zaman uyumsuz g/ç veya ağ çağrılar yaparsa.</span><span class="sxs-lookup"><span data-stu-id="a58aa-178">Implement this interface to perform asynchronous authorization logic; for example, if your authorization logic makes asynchronous I/O or network calls.</span></span> <span data-ttu-id="a58aa-179">(Yetkilendirme mantığınızı CPU bağımlı ise, türetilen daha kolaydır **AuthorizationFilterAttribute**, çünkü zaman uyumsuz bir yöntem yazma gerek yoktur.)</span><span class="sxs-lookup"><span data-stu-id="a58aa-179">(If your authorization logic is CPU-bound, it is simpler to derive from **AuthorizationFilterAttribute**, because then you don't need to write an asynchronous method.)</span></span>

<span data-ttu-id="a58aa-180">Aşağıdaki diyagramda için sınıf hiyerarşisi gösterilmektedir **AuthorizeAttribute** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a58aa-180">The following diagram shows the class hierarchy for the **AuthorizeAttribute** class.</span></span>

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a><span data-ttu-id="a58aa-181">Bir denetleyici eylemi içinde yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="a58aa-181">Authorization Inside a Controller Action</span></span>

<span data-ttu-id="a58aa-182">Bazı durumlarda, devam et, ancak üzerinde asıl dayalı davranışı değiştirmek için bir istek sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-182">In some cases, you might allow a request to proceed, but change the behavior based on the principal.</span></span> <span data-ttu-id="a58aa-183">Örneğin, geri bilgileri kullanıcı rolüne bağlı olarak değişebilir.</span><span class="sxs-lookup"><span data-stu-id="a58aa-183">For example, the information that you return might change depending on the user's role.</span></span> <span data-ttu-id="a58aa-184">Denetleyici yöntemi içinde gelen geçerli ilkesini alabilirsiniz **ApiController.User** özelliği.</span><span class="sxs-lookup"><span data-stu-id="a58aa-184">Within a controller method, you can get the current principle from the **ApiController.User** property.</span></span>

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
