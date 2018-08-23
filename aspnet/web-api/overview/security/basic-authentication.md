---
uid: web-api/overview/security/basic-authentication
title: ASP.NET Web API'de temel kimlik doğrulaması | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API'de temel kimlik doğrulaması kullanmayı açıklar.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7afafb6b7851f0d955d1f4292318f64d2a068a45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754139"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="371b9-103">ASP.NET Web API'de temel kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="371b9-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="371b9-104">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="371b9-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="371b9-105">Temel kimlik doğrulaması tanımlanmış [RFC 2617, HTTP kimlik doğrulaması: temel ve Özet erişimi kimlik doğrulaması](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="371b9-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="371b9-106">Dezavantajlar</span><span class="sxs-lookup"><span data-stu-id="371b9-106">Disadvantages</span></span>

- <span data-ttu-id="371b9-107">Kullanıcı kimlik bilgileri isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="371b9-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="371b9-108">Kimlik bilgileri düz metin olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="371b9-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="371b9-109">Kimlik bilgileri, her bir istekle gönderilir.</span><span class="sxs-lookup"><span data-stu-id="371b9-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="371b9-110">Yolu, oturum dışındaki tarayıcı oturumu sonlandırarak.</span><span class="sxs-lookup"><span data-stu-id="371b9-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="371b9-111">Siteler arası istek sahteciliği (CSRF); karşı savunmasız Anti-CSRF ölçülerin gerektirir.</span><span class="sxs-lookup"><span data-stu-id="371b9-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="371b9-112">Yararları</span><span class="sxs-lookup"><span data-stu-id="371b9-112">Advantages</span></span>

- <span data-ttu-id="371b9-113">Internet standart.</span><span class="sxs-lookup"><span data-stu-id="371b9-113">Internet standard.</span></span>
- <span data-ttu-id="371b9-114">Tüm bilinen tarayıcılar tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="371b9-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="371b9-115">Görece basit protokolü.</span><span class="sxs-lookup"><span data-stu-id="371b9-115">Relatively simple protocol.</span></span>

<span data-ttu-id="371b9-116">Temel kimlik doğrulaması gibi çalışır:</span><span class="sxs-lookup"><span data-stu-id="371b9-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="371b9-117">Sunucu, isteği kimlik doğrulaması gerektiriyorsa, 401 (yetkisiz) döndürür.</span><span class="sxs-lookup"><span data-stu-id="371b9-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="371b9-118">Yanıt, sunucunun temel kimlik doğrulamasını destekleyip belirten bir WWW-Authenticate üstbilgisi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="371b9-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="371b9-119">İstemci, yetkilendirme üst bilgisinde istemci kimlik bilgileri ile başka bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="371b9-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="371b9-120">Kimlik bilgileri "adı: parola", base64 ile kodlanmış dize olarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="371b9-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="371b9-121">Kimlik bilgileri şifrelenmez.</span><span class="sxs-lookup"><span data-stu-id="371b9-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="371b9-122">Temel kimlik doğrulaması, bir "realm." bağlamında yapılır</span><span class="sxs-lookup"><span data-stu-id="371b9-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="371b9-123">Sunucu, WWW-Authenticate üstbilgisi bölge adını içerir.</span><span class="sxs-lookup"><span data-stu-id="371b9-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="371b9-124">Kullanıcının kimlik bilgilerini bu bölge içinde geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="371b9-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="371b9-125">Bir bölge tam kapsamı, sunucu tarafından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="371b9-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="371b9-126">Örneğin sırada bölüm kaynakları için çeşitli bölgeleri tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="371b9-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="371b9-127">Kimlik bilgileri şifrelenmemiş gönderilir, temel kimlik doğrulaması yalnızca HTTPS üzerinden güvenli olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="371b9-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="371b9-128">Bkz: [Web API'de SSL ile çalışma](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="371b9-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="371b9-129">Temel kimlik doğrulaması da CSRF saldırılarına karşı savunmasız durumdadır.</span><span class="sxs-lookup"><span data-stu-id="371b9-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="371b9-130">Kullanıcı kimlik bilgilerini girdikten sonra tarayıcının otomatik olarak bunları sonraki isteklerde authenticateasync aynı etki alanına oturum süresi boyunca gönderir.</span><span class="sxs-lookup"><span data-stu-id="371b9-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="371b9-131">Bu AJAX istekleri içerir.</span><span class="sxs-lookup"><span data-stu-id="371b9-131">This includes AJAX requests.</span></span> <span data-ttu-id="371b9-132">Bkz: [siteler arası istek sahteciliği (CSRF) saldırılarını önleme](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="371b9-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="371b9-133">IIS ile temel kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="371b9-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="371b9-134">IIS temel kimlik doğrulamasını destekler, ancak bir uyarı yok: kullanıcı, Windows kimlik bilgilerine karşı kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="371b9-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="371b9-135">Kullanıcı, sunucunun etki alanında bir hesabınızın olması gerekir anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="371b9-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="371b9-136">Genel kullanıma yönelik bir web sitesi için genellikle bir ASP.NET üyelik sağlayıcı karşı kimlik doğrulaması istersiniz.</span><span class="sxs-lookup"><span data-stu-id="371b9-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="371b9-137">IIS kullanarak temel kimlik doğrulamasını etkinleştirmek için kimlik doğrulama modu, ASP.NET projesinin Web.config dosyasındaki "Windows" ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="371b9-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="371b9-138">Bu modda, IIS kimlik doğrulaması için Windows kimlik bilgilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="371b9-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="371b9-139">Ayrıca, IIS temel kimlik doğrulamasını etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="371b9-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="371b9-140">IIS Yöneticisi'nde özellikler görünümüne gidin, kimlik doğrulama yöntemini seçin ve temel kimlik doğrulamasını etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="371b9-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="371b9-141">Web API projesinde, ekleyin `[Authorize]` kimlik doğrulaması gerektiren herhangi bir denetleyici eylemleri için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="371b9-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="371b9-142">Bir istemci kendi yetkilendirme üst bilgisi istekte ayarlayarak kimliğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="371b9-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="371b9-143">Tarayıcı istemcilerinin, otomatik olarak bu adımı gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="371b9-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="371b9-144">Nonbrowser istemcileri üstbilgi ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="371b9-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="371b9-145">Özel üyelik ile temel kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="371b9-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="371b9-146">Belirtildiği gibi temel IIS ile yerleşik kimlik doğrulaması Windows kimlik bilgilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="371b9-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="371b9-147">Barındırma sunucusundaki kullanıcılarınız için hesapları oluşturmak için ihtiyacınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="371b9-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="371b9-148">Ancak bir Internet uygulaması için kullanıcı hesapları genellikle bir dış veritabanında depolanır.</span><span class="sxs-lookup"><span data-stu-id="371b9-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="371b9-149">Temel kimlik doğrulaması gerçekleştiren bir HTTP modülü nasıl aşağıdaki kod.</span><span class="sxs-lookup"><span data-stu-id="371b9-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="371b9-150">Bir ASP.NET üyelik sağlayıcısı değiştirerek kolayca takılabilir `CheckPassword` Bu örnekte işlevsiz bir yöntemi olan yöntem.</span><span class="sxs-lookup"><span data-stu-id="371b9-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="371b9-151">Web API 2'de yazma dikkate almanız gereken bir [kimlik doğrulaması filtresini](authentication-filters.md) veya [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/index.md), yerine bir HTTP modülü.</span><span class="sxs-lookup"><span data-stu-id="371b9-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="371b9-152">HTTP modülü etkinleştirmek için web.config dosyanızda şunları ekleyin **system.webServer** bölümü:</span><span class="sxs-lookup"><span data-stu-id="371b9-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="371b9-153">"YourAssemblyName" ("dll" uzantısı dahil değil) derlemenin adı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="371b9-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="371b9-154">Forms veya Windows kimlik doğrulaması gibi diğer kimlik doğrulama düzenleri devre dışı bırakmanız gerekir</span><span class="sxs-lookup"><span data-stu-id="371b9-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
