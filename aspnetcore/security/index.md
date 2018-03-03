---
title: "ASP.NET Core güvenliğine genel bakış"
author: rachelappel
description: "ASP.NET Core kimlik doğrulama, yetkilendirme ve güvenlik temel kavramları hakkında bilgi edinin."
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: 7e5f6bc44241dc6fc11569a145a04340f1b3ee7f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-security-overview"></a>ASP.NET Core güvenliğine genel bakış

ASP.NET Core geliştiricilerin kolayca yapılandırmanıza ve uygulamalarını güvenliğini yönetmenize olanak sağlar. ASP.NET Core kimlik doğrulama, yetkilendirme, veri koruma, SSL zorlama, uygulama parolaları, koruma istek sahteciliğine koruma ve CORS yönetim yönetmeye yönelik özellikler içerir. Bu güvenlik özellikleri, sağlam yapı henüz ASP.NET Core uygulamaları güvenli olanak tanır.

## <a name="aspnet-core-security-features"></a>ASP.NET Core güvenlik özellikleri

ASP.NET Core birçok araçları ve yerleşik kimlik sağlayıcıları gibi uygulamalarınızın güvenliğini sağlamak için kitaplıkları sağlar ancak 3 taraf kimlik hizmetlerini Facebook, Twitter ve LinkedIn gibi kullanabilirsiniz. ASP.NET Core ile depolamak ve kodda kullanıma gerek kalmadan gizli bilgileri kullanmak için bir yoldur uygulama sırrı kolayca yönetebilirsiniz.

## <a name="authentication-vs-authorization"></a>Kimlik doğrulama vs. Yetkilendirme

Kimlik doğrulaması, bir kullanıcı daha sonra bu bir işletim sistemi, veritabanı, uygulama veya kaynak depolanan karşılaştırılır kimlik bilgilerini sağlayan bir işlemdir. Eşleşirlerse, kullanıcıların kimliğini başarıyla doğrulamak ve, yetkili oldukları eylemler gerçekleştirebilir bir yetkilendirme sürecinde. Yetkilendirme kullanıcı yapmak için verileceğini belirler işleme başvuruyor.

Kimliğini düşünmeye başka bir kullanıcı (sunucu, veritabanı veya uygulamanın) alan içinde hangi nesneleri için hangi eylemleri olsa da yetkilendirme sunucusu, veritabanı, uygulama veya kaynak gibi bir alanı girmek için bir yol olarak dikkate alınması gereken yoludur.

## <a name="common-vulnerabilities-in-software"></a>Ortak Güvenlik Açıkları yazılım

ASP.NET Çekirdeği ve EF yardımcı özellikler içeriyor uygulamalarınızın güvenliğini sağlamak ve güvenlik açıklarına. Aşağıdaki listesi bağlantılar, web uygulamalarında en yaygın güvenlik açıkları önlemek için belgeleri ayrıntılandıran teknikleri alır:

* [Siteler arası komut dosyası saldırıları](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [SQL ekleme saldırıları](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Siteler arası istek sahtekarlığı (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Açık yeniden yönlendirme saldırılarına](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Bilmeniz gereken daha fazla güvenlik açıkları vardır. Daha fazla bilgi için bu belgedeki bölümüne bakarak *ASP.NET güvenlik belgelerine*.

## <a name="aspnet-security-documentation"></a>ASP.NET güvenlik belgeleri

*   [Kimlik Doğrulaması](authentication/index.md)
    *   [Kimliğe giriş](authentication/identity.md)
    *   [Facebook, Google ve diğer dış sağlayıcıları kullanarak kimlik doğrulamasını etkinleştirme](authentication/social/index.md)
    *   [WS-Federasyon ile kimlik doğrulamasını etkinleştir](authentication/ws-federation.md)
    * [Windows Kimlik Doğrulaması Yapılandırma](authentication/windowsauth.md)
    *   [Hesap onaylama ve parola kurtarma](authentication/accconfirm.md)
    *   [SMS ile iki öğeli kimlik doğrulama](authentication/2fa.md)
    *   [Kimlik olmadan tanımlama bilgisi kimlik doğrulamasını kullan](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Azure AD bir ASP.NET Core web uygulamanıza tümleştirmek](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Azure AD kullanarak bir WPF uygulamasından ASP.NET çekirdek Web API çağırma](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Azure AD kullanarak bir ASP.NET Core web uygulamasında Web API’si çağırma](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Azure AD B2C ile bir ASP.NET Core web uygulama](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [IdentityServer4 ile ASP.NET Core uygulamalarının güvenliğini sağlama](https://identityserver4.readthedocs.io)
*   [Yetkilendirme](authorization/index.md)
    *   [Giriş](authorization/introduction.md)
    *   [Kullanıcı verilerinin yetkilendirme tarafından korunduğu bir uygulama oluşturma](xref:security/authorization/secure-data)
    *   [Basit yetkilendirme](authorization/simple.md)
    *   [Rol tabanlı yetkilendirme](authorization/roles.md)
    *   [Talep tabanlı yetkilendirme](authorization/claims.md)
    *   [İlke tabanlı yetkilendirme](authorization/policies.md)
    *   [Gereksinim işleyicilerine bağımlılık ekleme](authorization/dependencyinjection.md)
    *   [Kaynak tabanlı yetkilendirme](authorization/resourcebased.md)
    *   [Görünüm tabanlı yetkilendirme](authorization/views.md)
    *   [Şemayla kimliği sınırlama](authorization/limitingidentitybyscheme.md)
*   [Veri koruma](data-protection/index.md)
    *   [Veri korumaya giriş](data-protection/introduction.md)
    *   [Veri Koruma API’lerini kullanmaya başlama](data-protection/using-data-protection.md)
    *   [Tüketici API'leri](data-protection/consumer-apis/index.md)
        *   [Tüketici API'lerine Genel Bakış](data-protection/consumer-apis/overview.md)
        *   [Amaç dizeleri](data-protection/consumer-apis/purpose-strings.md)
        *   [Amaç hiyerarşisi ve çok kiracılılık](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Parola karması](data-protection/consumer-apis/password-hashing.md)
        *   [Korumalı yüklerin ömrünü sınırlama](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Anahtarları iptal edilen yüklerin korumasını kaldırma](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Yapılandırma](data-protection/configuration/index.md)
        *   [Veri korumasını yapılandırma](data-protection/configuration/overview.md)
        *   [Varsayılan ayarlar](data-protection/configuration/default-settings.md)
        *   [Makine geneli ilke](data-protection/configuration/machine-wide-policy.md)
        *   [DI kullanan olmayan senaryolar](data-protection/configuration/non-di-scenarios.md)
    *   [Genişletilebilirlik API’leri](data-protection/extensibility/index.md)
        *   [Çekirdek şifreleme genişletilebilirliği](data-protection/extensibility/core-crypto.md)
        *   [Anahtar yönetimi genişletilebilirliği](data-protection/extensibility/key-management.md)
        *   [Çeşitli API'ler](data-protection/extensibility/misc-apis.md)
    *   [Uygulama](data-protection/implementation/index.md)
        *   [Kimliği doğrulanmış şifreleme ayrıntıları](data-protection/implementation/authenticated-encryption-details.md)
        *   [Alt anahtar türetme ve kimliği doğrulanmış şifreleme](data-protection/implementation/subkeyderivation.md)
        *   [Bağlam üst bilgileri](data-protection/implementation/context-headers.md)
        *   [Anahtar yönetimi](data-protection/implementation/key-management.md)
        *   [Anahtar depolama sağlayıcıları](data-protection/implementation/key-storage-providers.md)
        *   [Bekleme durumunda anahtar şifreleme](data-protection/implementation/key-encryption-at-rest.md)
        *   [Anahtar değiştirilemezliği ve ayarları değiştirme](data-protection/implementation/key-immutability.md)
        *   [Anahtar depolama biçimi](data-protection/implementation/key-storage-format.md)
        *   [Kısa ömürlü veri koruma sağlayıcıları](data-protection/implementation/key-storage-ephemeral.md)
    *   [Uyumluluk](data-protection/compatibility/index.md)
        *   [Uygulamalar arasında tanımlama bilgilerini paylaşma](data-protection/compatibility/cookie-sharing.md)
        *   [ASP.NET’te <machineKey> değiştirme](data-protection/compatibility/replacing-machinekey.md)
*   [Kullanıcı verilerinin yetkilendirme tarafından korunduğu bir uygulama oluşturma](xref:security/authorization/secure-data)
*   [Geliştirme sırasında uygulama gizli anahtarlarının güvenli bir şekilde depolanması](app-secrets.md)
*   [Azure Key Vault yapılandırma sağlayıcısı](key-vault-configuration.md)
*   [SSL uygulama](enforcing-ssl.md)
*   [İstek Sahteciliğinden Koruma](anti-request-forgery.md)
*   [Açık yeniden yönlendirme saldırılarını önleme](preventing-open-redirects.md)
*   [Siteler Arası Betik kullanmayı önleme](cross-site-scripting.md)
*   [Kaynaklar Arası İstekleri (CORS) etkinleştirme](cors.md)
