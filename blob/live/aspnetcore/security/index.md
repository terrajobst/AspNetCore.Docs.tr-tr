---
title: "ASP.NET Core güvenliğine genel bakış | Microsoft Docs"
author: rachelappel
description: "ASP.NET Core kimlik doğrulama, yetkilendirme ve güvenlik temel kavramları hakkında bilgi edinin"
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: 71bde77e0bc5698b670b560455301cae642165a6
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-security-overview"></a><span data-ttu-id="97a7d-103">ASP.NET Core güvenliğine genel bakış</span><span class="sxs-lookup"><span data-stu-id="97a7d-103">ASP.NET Core Security Overview</span></span>

<span data-ttu-id="97a7d-104">ASP.NET Core geliştiricilerin kolayca yapılandırmanıza ve uygulamalarını güvenliğini yönetmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="97a7d-104">ASP.NET Core enables developers to easily configure and manage security for their apps.</span></span> <span data-ttu-id="97a7d-105">ASP.NET Core kimlik doğrulama, yetkilendirme, veri koruma, SSL zorlama, uygulama parolaları, koruma istek sahteciliğine koruma ve CORS yönetim yönetmeye yönelik özellikler içerir.</span><span class="sxs-lookup"><span data-stu-id="97a7d-105">ASP.NET Core contains features for managing authentication, authorization, data protection, SSL enforcement, app secrets, anti-request forgery protection, and CORS management.</span></span> <span data-ttu-id="97a7d-106">Bu güvenlik özellikleri, sağlam yapı henüz ASP.NET Core uygulamaları güvenli olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="97a7d-106">These security features allow you to build robust yet secure ASP.NET Core apps.</span></span> 

## <a name="aspnet-core-security-features"></a><span data-ttu-id="97a7d-107">ASP.NET Core güvenlik özellikleri</span><span class="sxs-lookup"><span data-stu-id="97a7d-107">ASP.NET Core security features</span></span>

<span data-ttu-id="97a7d-108">ASP.NET Core birçok araçları ve yerleşik kimlik sağlayıcıları gibi uygulamalarınızın güvenliğini sağlamak için kitaplıkları sağlar ancak 3 taraf kimlik hizmetlerini Facebook, Twitter ve LinkedIn gibi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97a7d-108">ASP.NET Core provides many tools and libraries to secure your apps including built-in Identity providers but you can use 3rd party identity services such as Facebook, Twitter, or LinkedIn.</span></span> <span data-ttu-id="97a7d-109">ASP.NET Core ile depolamak ve kodda kullanıma gerek kalmadan gizli bilgileri kullanmak için bir yoldur uygulama sırrı kolayca yönetebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="97a7d-109">With ASP.NET Core, you can easily manage app secrets, which are a way to store and use confidential information without having to expose it in the code.</span></span> 

## <a name="authentication-vs-authorization"></a><span data-ttu-id="97a7d-110">Kimlik doğrulama vs. Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-110">Authentication vs. Authorization</span></span>

<span data-ttu-id="97a7d-111">Kimlik doğrulaması, bir kullanıcı daha sonra bu bir işletim sistemi, veritabanı, uygulama veya kaynak depolanan karşılaştırılır kimlik bilgilerini sağlayan bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="97a7d-111">Authentication is a process in which a user provides credentials that are then compared to those stored in an operating system, database, app or resource.</span></span> <span data-ttu-id="97a7d-112">Eşleşirlerse, kullanıcıların kimliğini başarıyla doğrulamak ve, yetkili eylemler gerçekleştirebilir bir yetkilendirme sürecinde.</span><span class="sxs-lookup"><span data-stu-id="97a7d-112">If they match, users authenticate successfully, and can then perform actions that they are authorized for, during an authorization process.</span></span> <span data-ttu-id="97a7d-113">Yetkilendirme kullanıcı yapmak için verileceğini belirler işleme başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="97a7d-113">The authorization refers to the process that determines what a user is allowed to do.</span></span> 

<span data-ttu-id="97a7d-114">Kimliğini düşünmeye başka bir kullanıcı (sunucu, veritabanı veya uygulamanın) alan içinde hangi nesneleri için hangi eylemleri olsa da yetkilendirme sunucusu, veritabanı, uygulama veya kaynak gibi bir alanı girmek için bir yol olarak dikkate alınması gereken yoludur.</span><span class="sxs-lookup"><span data-stu-id="97a7d-114">Another way to think of authentication is to consider it as a way to enter a space, such as a server, database, app or resource, while authorization is which actions the user can perform to which objects inside that space (server, database, or app).</span></span>

## <a name="common-vulnerabilities-in-software"></a><span data-ttu-id="97a7d-115">Ortak Güvenlik Açıkları yazılım</span><span class="sxs-lookup"><span data-stu-id="97a7d-115">Common Vulnerabilities in software</span></span>

<span data-ttu-id="97a7d-116">ASP.NET Çekirdeği ve EF yardımcı özellikler içeriyor uygulamalarınızın güvenliğini sağlamak ve güvenlik açıklarına.</span><span class="sxs-lookup"><span data-stu-id="97a7d-116">ASP.NET Core and EF contain features that help you secure your apps and prevent security breaches.</span></span> <span data-ttu-id="97a7d-117">Aşağıdaki listesi bağlantılar, web uygulamalarında en yaygın güvenlik açıkları önlemek için belgeleri ayrıntılandıran teknikleri alır:</span><span class="sxs-lookup"><span data-stu-id="97a7d-117">The following list of links takes you to documentation detailing techniques to avoid the most common security vulnerabilities in web apps:</span></span>

* [<span data-ttu-id="97a7d-118">Siteler arası komut dosyası saldırıları</span><span class="sxs-lookup"><span data-stu-id="97a7d-118">Cross-site scripting attacks</span></span>](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [<span data-ttu-id="97a7d-119">SQL ekleme saldırıları</span><span class="sxs-lookup"><span data-stu-id="97a7d-119">SQL injection attacks</span></span>](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [<span data-ttu-id="97a7d-120">Siteler arası istek sahtekarlığı (CSRF)</span><span class="sxs-lookup"><span data-stu-id="97a7d-120">Cross-Site Request Forgery (CSRF)</span></span>](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [<span data-ttu-id="97a7d-121">Açık yeniden yönlendirme saldırılarına</span><span class="sxs-lookup"><span data-stu-id="97a7d-121">Open redirect attacks</span></span>](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

<span data-ttu-id="97a7d-122">Bilmeniz gereken daha fazla güvenlik açıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="97a7d-122">There are more vulnerabilities that you should be aware of.</span></span> <span data-ttu-id="97a7d-123">Daha fazla bilgi için bu belgedeki bölümüne bakarak *ASP.NET güvenlik belgelerine*.</span><span class="sxs-lookup"><span data-stu-id="97a7d-123">For more information, see the section in this document on *ASP.NET Security Documentation*.</span></span> 

## <a name="aspnet-security-documentation"></a><span data-ttu-id="97a7d-124">ASP.NET güvenlik belgeleri</span><span class="sxs-lookup"><span data-stu-id="97a7d-124">ASP.NET Security Documentation</span></span>

*   [<span data-ttu-id="97a7d-125">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="97a7d-125">Authentication</span></span>](authentication/index.md)
    *   [<span data-ttu-id="97a7d-126">Kimliğe giriş</span><span class="sxs-lookup"><span data-stu-id="97a7d-126">Introduction to Identity</span></span>](authentication/identity.md)
    *   [<span data-ttu-id="97a7d-127">Facebook, Google ve diğer dış sağlayıcıları kullanarak kimlik doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-127">Enable authentication using Facebook, Google, and other external providers</span></span>](authentication/social/index.md)
    * [<span data-ttu-id="97a7d-128">Windows Kimlik Doğrulaması Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="97a7d-128">Configure Windows Authentication</span></span>](authentication/windowsauth.md)
    *   [<span data-ttu-id="97a7d-129">Hesap onaylama ve parola kurtarma</span><span class="sxs-lookup"><span data-stu-id="97a7d-129">Account confirmation and password recovery</span></span>](authentication/accconfirm.md)
    *   [<span data-ttu-id="97a7d-130">SMS ile iki öğeli kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="97a7d-130">Two-factor authentication with SMS</span></span>](authentication/2fa.md) 
    *   [<span data-ttu-id="97a7d-131">Kimlik olmadan tanımlama bilgisi kimlik doğrulamasını kullan</span><span class="sxs-lookup"><span data-stu-id="97a7d-131">Use cookie authentication without Identity</span></span>](authentication/cookie.md)
    *   [<span data-ttu-id="97a7d-132">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="97a7d-132">Azure Active Directory</span></span>](authentication/azure-active-directory/index.md)
        *   [<span data-ttu-id="97a7d-133">Azure AD bir ASP.NET Core web uygulamanıza tümleştirmek</span><span class="sxs-lookup"><span data-stu-id="97a7d-133">Integrate Azure AD into an ASP.NET Core web app</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="97a7d-134">Azure AD kullanarak bir WPF uygulamasından ASP.NET çekirdek Web API çağırma</span><span class="sxs-lookup"><span data-stu-id="97a7d-134">Call an ASP.NET Core Web API from a WPF app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [<span data-ttu-id="97a7d-135">Azure AD kullanarak bir ASP.NET Core web uygulamasında Web API’si çağırma</span><span class="sxs-lookup"><span data-stu-id="97a7d-135">Call a Web API in an ASP.NET Core web app using Azure AD</span></span>](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [<span data-ttu-id="97a7d-136">Azure AD B2C ile bir ASP.NET Core web uygulama</span><span class="sxs-lookup"><span data-stu-id="97a7d-136">An ASP.NET Core web app with Azure AD B2C</span></span>](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [<span data-ttu-id="97a7d-137">IdentityServer4 ile ASP.NET Core uygulamalarının güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="97a7d-137">Secure ASP.NET Core apps with IdentityServer4</span></span>](https://identityserver4.readthedocs.io)
*   [<span data-ttu-id="97a7d-138">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-138">Authorization</span></span>](authorization/index.md)
    *   [<span data-ttu-id="97a7d-139">Giriş</span><span class="sxs-lookup"><span data-stu-id="97a7d-139">Introduction</span></span>](authorization/introduction.md)
    *   [<span data-ttu-id="97a7d-140">Kullanıcı verilerinin yetkilendirme tarafından korunduğu bir uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="97a7d-140">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
    *   [<span data-ttu-id="97a7d-141">Basit yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-141">Simple authorization</span></span>](authorization/simple.md)
    *   [<span data-ttu-id="97a7d-142">Rol tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-142">Role-based authorization</span></span>](authorization/roles.md)
    *   [<span data-ttu-id="97a7d-143">Talep tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-143">Claims-based authorization</span></span>](authorization/claims.md)
    *   [<span data-ttu-id="97a7d-144">İlke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-144">Policy-based authorization</span></span>](authorization/policies.md)
    *   [<span data-ttu-id="97a7d-145">Gereksinim işleyicilerine bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="97a7d-145">Dependency injection in requirement handlers</span></span>](authorization/dependencyinjection.md)
    *   [<span data-ttu-id="97a7d-146">Kaynak tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-146">Resource-based authorization</span></span>](authorization/resourcebased.md)
    *   [<span data-ttu-id="97a7d-147">Görünüm tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-147">View-based authorization</span></span>](authorization/views.md)
    *   [<span data-ttu-id="97a7d-148">Şemayla kimliği sınırlama</span><span class="sxs-lookup"><span data-stu-id="97a7d-148">Limit identity by scheme</span></span>](authorization/limitingidentitybyscheme.md)
*   [<span data-ttu-id="97a7d-149">Veri koruma</span><span class="sxs-lookup"><span data-stu-id="97a7d-149">Data protection</span></span>](data-protection/index.md)
    *   [<span data-ttu-id="97a7d-150">Veri korumaya giriş</span><span class="sxs-lookup"><span data-stu-id="97a7d-150">Introduction to data protection</span></span>](data-protection/introduction.md)
    *   [<span data-ttu-id="97a7d-151">Veri Koruma API’lerini kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="97a7d-151">Get started with the Data Protection APIs</span></span>](data-protection/using-data-protection.md)
    *   [<span data-ttu-id="97a7d-152">Tüketici API'leri</span><span class="sxs-lookup"><span data-stu-id="97a7d-152">Consumer APIs</span></span>](data-protection/consumer-apis/index.md)
        *   [<span data-ttu-id="97a7d-153">Tüketici API'lerine Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="97a7d-153">Consumer APIs Overview</span></span>](data-protection/consumer-apis/overview.md)
        *   [<span data-ttu-id="97a7d-154">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="97a7d-154">Purpose strings</span></span>](data-protection/consumer-apis/purpose-strings.md)
        *   [<span data-ttu-id="97a7d-155">Amaç hiyerarşisi ve çok kiracılılık</span><span class="sxs-lookup"><span data-stu-id="97a7d-155">Purpose hierarchy and multi-tenancy</span></span>](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [<span data-ttu-id="97a7d-156">Parola karması</span><span class="sxs-lookup"><span data-stu-id="97a7d-156">Password hashing</span></span>](data-protection/consumer-apis/password-hashing.md)
        *   [<span data-ttu-id="97a7d-157">Korumalı yüklerin ömrünü sınırlama</span><span class="sxs-lookup"><span data-stu-id="97a7d-157">Limit the lifetime of protected payloads</span></span>](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [<span data-ttu-id="97a7d-158">Anahtarları iptal edilen yüklerin korumasını kaldırma</span><span class="sxs-lookup"><span data-stu-id="97a7d-158">Unprotect payloads whose keys have been revoked</span></span>](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [<span data-ttu-id="97a7d-159">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="97a7d-159">Configuration</span></span>](data-protection/configuration/index.md)
        *   [<span data-ttu-id="97a7d-160">Veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="97a7d-160">Configure data protection</span></span>](data-protection/configuration/overview.md)
        *   [<span data-ttu-id="97a7d-161">Varsayılan ayarlar</span><span class="sxs-lookup"><span data-stu-id="97a7d-161">Default settings</span></span>](data-protection/configuration/default-settings.md)
        *   [<span data-ttu-id="97a7d-162">Makine geneli ilke</span><span class="sxs-lookup"><span data-stu-id="97a7d-162">Machine-wide policy</span></span>](data-protection/configuration/machine-wide-policy.md)
        *   [<span data-ttu-id="97a7d-163">DI kullanan olmayan senaryolar</span><span class="sxs-lookup"><span data-stu-id="97a7d-163">Non DI-aware scenarios</span></span>](data-protection/configuration/non-di-scenarios.md)
    *   [<span data-ttu-id="97a7d-164">Genişletilebilirlik API’leri</span><span class="sxs-lookup"><span data-stu-id="97a7d-164">Extensibility APIs</span></span>](data-protection/extensibility/index.md)
        *   [<span data-ttu-id="97a7d-165">Çekirdek şifreleme genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="97a7d-165">Core cryptography extensibility</span></span>](data-protection/extensibility/core-crypto.md)
        *   [<span data-ttu-id="97a7d-166">Anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="97a7d-166">Key management extensibility</span></span>](data-protection/extensibility/key-management.md)
        *   [<span data-ttu-id="97a7d-167">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="97a7d-167">Miscellaneous APIs</span></span>](data-protection/extensibility/misc-apis.md)
    *   [<span data-ttu-id="97a7d-168">Uygulama</span><span class="sxs-lookup"><span data-stu-id="97a7d-168">Implementation</span></span>](data-protection/implementation/index.md)
        *   [<span data-ttu-id="97a7d-169">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="97a7d-169">Authenticated encryption details</span></span>](data-protection/implementation/authenticated-encryption-details.md)
        *   [<span data-ttu-id="97a7d-170">Alt anahtar türetme ve kimliği doğrulanmış şifreleme</span><span class="sxs-lookup"><span data-stu-id="97a7d-170">Subkey derivation and authenticated encryption</span></span>](data-protection/implementation/subkeyderivation.md)
        *   [<span data-ttu-id="97a7d-171">Bağlam üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="97a7d-171">Context headers</span></span>](data-protection/implementation/context-headers.md)
        *   [<span data-ttu-id="97a7d-172">Anahtar yönetimi</span><span class="sxs-lookup"><span data-stu-id="97a7d-172">Key management</span></span>](data-protection/implementation/key-management.md)
        *   [<span data-ttu-id="97a7d-173">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="97a7d-173">Key storage providers</span></span>](data-protection/implementation/key-storage-providers.md)
        *   [<span data-ttu-id="97a7d-174">Bekleme durumunda anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="97a7d-174">Key encryption at rest</span></span>](data-protection/implementation/key-encryption-at-rest.md)
        *   [<span data-ttu-id="97a7d-175">Anahtar değiştirilemezliği ve ayarları değiştirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-175">Key immutability and changing settings</span></span>](data-protection/implementation/key-immutability.md)
        *   [<span data-ttu-id="97a7d-176">Anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="97a7d-176">Key storage format</span></span>](data-protection/implementation/key-storage-format.md)
        *   [<span data-ttu-id="97a7d-177">Kısa ömürlü veri koruma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="97a7d-177">Ephemeral data protection providers</span></span>](data-protection/implementation/key-storage-ephemeral.md)
    *   [<span data-ttu-id="97a7d-178">Uyumluluk</span><span class="sxs-lookup"><span data-stu-id="97a7d-178">Compatibility</span></span>](data-protection/compatibility/index.md)
        *   [<span data-ttu-id="97a7d-179">Tanımlama bilgilerini uygulamalar arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="97a7d-179">Share cookies between apps</span></span>](data-protection/compatibility/cookie-sharing.md)
        *   [<span data-ttu-id="97a7d-180">ASP.NET’te <machineKey> değiştirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-180">Replace <machineKey> in ASP.NET</span></span>](data-protection/compatibility/replacing-machinekey.md)
*   [<span data-ttu-id="97a7d-181">Kullanıcı verilerinin yetkilendirme tarafından korunduğu bir uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="97a7d-181">Create an app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
*   [<span data-ttu-id="97a7d-182">Geliştirme sırasında uygulama gizli anahtarlarının güvenli bir şekilde depolanması</span><span class="sxs-lookup"><span data-stu-id="97a7d-182">Safe storage of app secrets during development</span></span>](app-secrets.md)
*   [<span data-ttu-id="97a7d-183">Azure Key Vault yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="97a7d-183">Azure Key Vault configuration provider</span></span>](key-vault-configuration.md)
*   [<span data-ttu-id="97a7d-184">SSL uygulama</span><span class="sxs-lookup"><span data-stu-id="97a7d-184">Enforce SSL</span></span>](enforcing-ssl.md)
*   [<span data-ttu-id="97a7d-185">İstek Sahteciliğinden Koruma</span><span class="sxs-lookup"><span data-stu-id="97a7d-185">Anti-Request Forgery</span></span>](anti-request-forgery.md)
*   [<span data-ttu-id="97a7d-186">Açık yeniden yönlendirme saldırılarını önleme</span><span class="sxs-lookup"><span data-stu-id="97a7d-186">Prevent open redirect attacks</span></span>](preventing-open-redirects.md)
*   [<span data-ttu-id="97a7d-187">Siteler Arası Betik kullanmayı önleme</span><span class="sxs-lookup"><span data-stu-id="97a7d-187">Prevent Cross-Site Scripting</span></span>](cross-site-scripting.md)
*   [<span data-ttu-id="97a7d-188">Kaynaklar Arası İstekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="97a7d-188">Enable Cross-Origin Requests (CORS)</span></span>](cors.md)