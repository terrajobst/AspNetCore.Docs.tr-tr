---
title: ASP.NET Core güvenliğine genel bakış
author: rachelappel
description: ASP.NET Core kimlik doğrulama, yetkilendirme ve güvenlik temel kavramları hakkında bilgi edinin.
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: da3829b2d5ae5db1861c7423da5afc7acbee6697
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="overview-of-aspnet-core-security"></a>ASP.NET Core güvenliğine genel bakış

ASP.NET Core geliştiricilerin kolayca yapılandırmanıza ve uygulamalarını güvenliğini yönetmenize olanak sağlar. ASP.NET Core kimlik doğrulama, yetkilendirme, veri koruma, SSL zorlama, uygulama parolaları, koruma istek sahteciliğine koruma ve CORS yönetim yönetmeye yönelik özellikler içerir. Bu güvenlik özellikleri, sağlam yapı henüz ASP.NET Core uygulamaları güvenli olanak tanır.

## <a name="aspnet-core-security-features"></a>ASP.NET Core güvenlik özellikleri

ASP.NET Core birçok araçları ve yerleşik kimlik sağlayıcıları gibi uygulamalarınızın güvenliğini sağlamak için kitaplıkları sağlar ancak 3 taraf kimlik hizmetlerini Facebook, Twitter ve LinkedIn gibi kullanabilirsiniz. ASP.NET Core ile depolamak ve kodda kullanıma gerek kalmadan gizli bilgileri kullanmak için bir yoldur uygulama sırrı kolayca yönetebilirsiniz.

## <a name="authentication-vs-authorization"></a>Kimlik doğrulama vs. Yetkilendirme

Kimlik doğrulaması, bir kullanıcı daha sonra bu bir işletim sistemi, veritabanı, uygulama veya kaynak depolanan karşılaştırılır kimlik bilgilerini sağlayan bir işlemdir. Eşleşirlerse, kullanıcıların kimliğini başarıyla doğrulamak ve, yetkili oldukları eylemler gerçekleştirebilir bir yetkilendirme sürecinde. Yetkilendirme kullanıcı yapmak için verileceğini belirler işleme başvuruyor.

Kimliğini düşünmeye başka bir kullanıcı (sunucu, veritabanı veya uygulamanın) alan içinde hangi nesneleri için hangi eylemleri olsa da yetkilendirme sunucusu, veritabanı, uygulama veya kaynak gibi bir alanı girmek için bir yol olarak dikkate alınması gereken yoludur.

## <a name="common-vulnerabilities-in-software"></a>Ortak Güvenlik Açıkları yazılım

ASP.NET Çekirdeği ve EF yardımcı özellikler içeriyor uygulamalarınızın güvenliğini sağlamak ve güvenlik açıklarına. Aşağıdaki listesi bağlantılar, web uygulamalarında en yaygın güvenlik açıkları önlemek için belgeleri ayrıntılandıran teknikleri alır:

* [Siteler arası komut dosyası saldırıları](xref:security/cross-site-scripting)
* [SQL ekleme saldırıları](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Siteler arası istek sahtekarlığı (CSRF)](xref:security/anti-request-forgery)
* [Açık yeniden yönlendirme saldırılarına](xref:security/preventing-open-redirects)

Bilmeniz gereken daha fazla güvenlik açıkları vardır. Daha fazla bilgi için bu belgedeki bölümüne bakarak *ASP.NET güvenlik belgelerine*.

## <a name="aspnet-security-documentation"></a>ASP.NET güvenlik belgeleri

*   [Kimlik Doğrulaması](xref:security/authentication/index)
    *   [Kimliğe giriş](xref:security/authentication/identity)
    *   [Facebook, Google ve diğer dış sağlayıcıları kullanarak kimlik doğrulamasını etkinleştirme](xref:security/authentication/social/index)
    *   [WS-Federasyon ile kimlik doğrulamasını etkinleştir](xref:security/authentication/ws-federation)
    * [Windows Kimlik Doğrulaması Yapılandırma](xref:security/authentication/windowsauth)
    *   [Hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm)
    *   [SMS ile iki öğeli kimlik doğrulama](xref:security/authentication/2fa)
    *   [Kimlik olmadan tanımlama bilgisi kimlik doğrulamasını kullan](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Azure AD bir ASP.NET Core web uygulamanıza tümleştirmek](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Azure AD kullanarak bir WPF uygulamasından ASP.NET çekirdek Web API çağırma](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Azure AD kullanarak bir ASP.NET Core web uygulamasında Web API’si çağırma](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Azure AD B2C ile bir ASP.NET Core web uygulama](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [IdentityServer4 ile ASP.NET Core uygulamalarının güvenliğini sağlama](https://identityserver4.readthedocs.io)
*   [Yetkilendirme](xref:security/authorization/index)
    *   [Giriş](xref:security/authorization/introduction)
    *   [Kullanıcı verilerinin yetkilendirme tarafından korunduğu bir uygulama oluşturma](xref:security/authorization/secure-data)
    *   [Basit yetkilendirme](xref:security/authorization/simple)
    *   [Rol tabanlı yetkilendirme](xref:security/authorization/roles)
    *   [Talep tabanlı yetkilendirme](xref:security/authorization/claims)
    *   [İlke tabanlı yetkilendirme](xref:security/authorization/policies)
    *   [Gereksinim işleyicilerine bağımlılık ekleme](xref:security/authorization/dependencyinjection)
    *   [Kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased)
    *   [Görünüm tabanlı yetkilendirme](xref:security/authorization/views)
    *   [Şemayla kimliği sınırlama](xref:security/authorization/limitingidentitybyscheme)
*   [Veri koruma](xref:security/data-protection/index)
    *   [Veri korumaya giriş](xref:security/data-protection/introduction)
    *   [Veri Koruma API’lerini kullanmaya başlama](xref:security/data-protection/using-data-protection)
    *   [Tüketici API'leri](xref:security/data-protection/consumer-apis/index)
        *   [Tüketici API'lerine Genel Bakış](xref:security/data-protection/consumer-apis/overview)
        *   [Amaç dizeleri](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [Amaç hiyerarşisi ve çok kiracılılık](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [Karma parolalar](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Korumalı yüklerin ömrünü sınırlama](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Anahtarları iptal edilen yüklerin korumasını kaldırma](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Yapılandırma](xref:security/data-protection/configuration/index)
        *   [Veri korumasını yapılandırma](xref:security/data-protection/configuration/overview)
        *   [Varsayılan ayarlar](xref:security/data-protection/configuration/default-settings)
        *   [Makine geneli ilke](xref:security/data-protection/configuration/machine-wide-policy)
        *   [DI kullanan olmayan senaryolar](xref:security/data-protection/configuration/non-di-scenarios)
    *   [Genişletilebilirlik API’leri](xref:security/data-protection/extensibility/index)
        *   [Çekirdek şifreleme genişletilebilirliği](xref:security/data-protection/extensibility/core-crypto)
        *   [Anahtar yönetimi genişletilebilirliği](xref:security/data-protection/extensibility/key-management)
        *   [Çeşitli API'ler](xref:security/data-protection/extensibility/misc-apis)
    *   [Uygulama](xref:security/data-protection/implementation/index)
        *   [Kimliği doğrulanmış şifreleme ayrıntıları](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [Alt anahtar türetme ve kimliği doğrulanmış şifreleme](xref:security/data-protection/implementation/subkeyderivation)
        *   [Bağlam üst bilgileri](xref:security/data-protection/implementation/context-headers)
        *   [Anahtar yönetimi](xref:security/data-protection/implementation/key-management)
        *   [Anahtar depolama sağlayıcıları](xref:security/data-protection/implementation/key-storage-providers)
        *   [Bekleme durumunda anahtar şifreleme](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [Anahtar girişi ve ayarları](xref:security/data-protection/implementation/key-immutability)
        *   [Anahtar depolama biçimi](xref:security/data-protection/implementation/key-storage-format)
        *   [Kısa ömürlü veri koruma sağlayıcıları](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Uyumluluk](xref:security/data-protection/compatibility/index)
        *   [ASP.NET’te <machineKey> değiştirme](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Kullanıcı verilerinin yetkilendirme tarafından korunduğu bir uygulama oluşturma](xref:security/authorization/secure-data)
*   [Güvenli Depolama Uygulama sırrı geliştirme](xref:security/app-secrets)
*   [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration)
*   [SSL uygulama](xref:security/enforcing-ssl)
*   [İstek Sahteciliğinden Koruma](xref:security/anti-request-forgery)
*   [Açık yeniden yönlendirme saldırılarını önleme](xref:security/preventing-open-redirects)
*   [Siteler Arası Betik kullanmayı önleme](xref:security/cross-site-scripting)
*   [Kaynaklar Arası İstekleri (CORS) etkinleştirme](xref:security/cors)
*   [Uygulamalar arasında tanımlama bilgilerini paylaşma](xref:security/cookie-sharing)
