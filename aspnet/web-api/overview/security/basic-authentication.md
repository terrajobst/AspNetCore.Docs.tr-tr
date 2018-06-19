---
uid: web-api/overview/security/basic-authentication
title: ASP.NET Web API'de temel kimlik doğrulaması | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API'de temel kimlik doğrulaması kullanmayı açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566745"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="4ce85-103">ASP.NET Web API'de temel kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="4ce85-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="4ce85-104">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4ce85-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4ce85-105">Temel kimlik doğrulaması tanımlanmış [RFC 2617, HTTP kimlik doğrulaması: temel ve Özet erişimi kimlik doğrulaması](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="4ce85-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="4ce85-106">Dezavantajlar</span><span class="sxs-lookup"><span data-stu-id="4ce85-106">Disadvantages</span></span>

- <span data-ttu-id="4ce85-107">Kullanıcı kimlik bilgileri isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="4ce85-108">Kimlik bilgileri düz metin olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="4ce85-109">Kimlik bilgileri sahip her isteği gönderilir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="4ce85-110">Yolu, oturum dışındaki Tarayıcı oturumunu sona erdirme.</span><span class="sxs-lookup"><span data-stu-id="4ce85-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="4ce85-111">Karşı savunmasız siteler arası istek sahtekarlığı (CSRF); Anti-CSRF ölçüleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="4ce85-112">Yararları</span><span class="sxs-lookup"><span data-stu-id="4ce85-112">Advantages</span></span>

- <span data-ttu-id="4ce85-113">Internet standardı.</span><span class="sxs-lookup"><span data-stu-id="4ce85-113">Internet standard.</span></span>
- <span data-ttu-id="4ce85-114">Önde gelen tüm tarayıcılar tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="4ce85-115">Görece basit protokolü.</span><span class="sxs-lookup"><span data-stu-id="4ce85-115">Relatively simple protocol.</span></span>

<span data-ttu-id="4ce85-116">Temel kimlik doğrulaması gibi çalışır:</span><span class="sxs-lookup"><span data-stu-id="4ce85-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="4ce85-117">Bir isteği kimlik doğrulaması gerektiriyorsa, sunucunun 401 (yetkisiz) döndürür.</span><span class="sxs-lookup"><span data-stu-id="4ce85-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="4ce85-118">Yanıt, sunucunun temel kimlik doğrulamasını destekleyen belirten bir WWW-Authenticate üstbilgisi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="4ce85-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="4ce85-119">İstemci yetkilendirme üst istemci kimlik bilgilerine sahip başka bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="4ce85-120">Kimlik bilgileri "name: parola", base64 ile kodlanmış dize olarak biçimlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="4ce85-121">Kimlik bilgileri şifrelenmez.</span><span class="sxs-lookup"><span data-stu-id="4ce85-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="4ce85-122">Temel kimlik doğrulaması "bölge." bağlamında gerçekleştirilir</span><span class="sxs-lookup"><span data-stu-id="4ce85-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="4ce85-123">Sunucunun bölgesi için ad WWW-Authenticate üst bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="4ce85-124">Kullanıcının kimlik bilgileri, o bölge içinde geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="4ce85-125">Bir bölge tam kapsamı sunucu tarafından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="4ce85-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="4ce85-126">Örneğin, bölüm kaynaklara sırayla birkaç bölgeleri tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ce85-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="4ce85-127">Kimlik bilgileri şifrelenmemiş gönderildiğinden, temel kimlik doğrulaması yalnızca HTTPS üzerinden güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="4ce85-128">Bkz: [Web API'de SSL ile çalışma](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4ce85-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="4ce85-129">Temel kimlik doğrulaması da CSRF saldırılarına açıktır.</span><span class="sxs-lookup"><span data-stu-id="4ce85-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="4ce85-130">Kullanıcı kimlik bilgilerini girdikten sonra tarayıcı otomatik olarak bunları sonraki isteklerde aynı etki alanına oturum süresince gönderir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="4ce85-131">AJAX istekleri içerir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-131">This includes AJAX requests.</span></span> <span data-ttu-id="4ce85-132">Bkz: [siteler arası istek sahtekarlığı (CSRF) saldırılarını önleme](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="4ce85-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="4ce85-133">IIS ile temel kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="4ce85-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="4ce85-134">IIS temel kimlik doğrulamasını destekler, ancak bir uyarı yok: kullanıcı için Windows kimlik bilgilerini karşı kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="4ce85-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="4ce85-135">Kullanıcı hesabınız sunucunun etki alanına sahip olmalıdır anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="4ce85-136">Genel kullanıma yönelik web sitesi için genellikle bir ASP.NET üyelik sağlayıcısı karşı kimlik doğrulaması istiyor.</span><span class="sxs-lookup"><span data-stu-id="4ce85-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="4ce85-137">IIS kullanarak temel kimlik doğrulamasını etkinleştirmek için ASP.NET projesi Web.config dosyasında "Windows" için kimlik doğrulama modunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="4ce85-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="4ce85-138">Bu modda, IIS kimlik doğrulaması için Windows kimlik bilgilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4ce85-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="4ce85-139">Ayrıca, IIS'de temel kimlik doğrulamasını etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="4ce85-140">IIS Yöneticisi ' nde özellikler görünümüne gidin, kimlik doğrulama yöntemini seçin ve temel kimlik doğrulamasını etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="4ce85-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="4ce85-141">Web API projenize eklemek `[Authorize]` kimlik doğrulaması gerektiren herhangi bir denetleyici eylemleri özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4ce85-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="4ce85-142">Bir istemci kendi istekte Authorization Üstbilgisi ayarlayarak kimliğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="4ce85-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="4ce85-143">Tarayıcı istemcileri otomatik olarak bu adımı gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="4ce85-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="4ce85-144">Nonbrowser istemcilerini üstbilgi ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="4ce85-145">Özel üyeliği ile temel kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="4ce85-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="4ce85-146">Belirtildiği gibi temel IIS içinde yerleşik olarak kimlik doğrulaması Windows kimlik bilgilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4ce85-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="4ce85-147">Barındırma sunucusundaki kullanıcılarınız için hesapları oluşturmanıza gerek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4ce85-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="4ce85-148">Ancak bir internet uygulama için kullanıcı hesapları genellikle bir dış veritabanında depolanır.</span><span class="sxs-lookup"><span data-stu-id="4ce85-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="4ce85-149">Temel kimlik doğrulaması gerçekleştiren bir HTTP modülü nasıl aşağıdaki kod.</span><span class="sxs-lookup"><span data-stu-id="4ce85-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="4ce85-150">Bir ASP.NET üyelik sağlayıcısı değiştirerek kolayca ekleyebilirsiniz `CheckPassword` Bu örnekte kukla bir yöntemdir yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4ce85-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="4ce85-151">Web API 2'de yazma dikkate almanız gereken bir [kimlik doğrulaması filtresini](authentication-filters.md) veya [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/index.md), HTTP modülü yerine.</span><span class="sxs-lookup"><span data-stu-id="4ce85-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="4ce85-152">HTTP modülü etkinleştirmek için web.config dosyanızda şunları ekleyin **system.webServer** bölümü:</span><span class="sxs-lookup"><span data-stu-id="4ce85-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="4ce85-153">"YourAssemblyName" ("dll" uzantısı dahil değil) derleme adı ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="4ce85-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="4ce85-154">Formlar veya Windows kimlik doğrulaması gibi diğer kimlik doğrulama şemasını devre dışı bırakmanız gerekir</span><span class="sxs-lookup"><span data-stu-id="4ce85-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
