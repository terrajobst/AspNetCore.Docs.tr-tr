---
title: ASP.NET Core güvenliğine genel bakış
author: tdykstra
description: ASP.NET Core kimlik doğrulaması, yetkilendirme ve güvenlik temel bilgileri öğrenin.
ms.author: tdykstra
ms.date: 11/01/2017
uid: security/index
ms.openlocfilehash: 3a1c1ea1ad28fccbe5ae91b0be193938b095f60b
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754508"
---
# <a name="overview-of-aspnet-core-security"></a>ASP.NET Core güvenliğine genel bakış

ASP.NET Core geliştiricilerinin kolayca yapılandırmak ve uygulamalarını için güvenliği yönetmek etkinleştirir. ASP.NET Core kimlik doğrulaması, yetkilendirme, veri koruma, SSL zorlama, uygulama gizli anahtarlarının, istek sahteciliğinden koruma koruması ve CORS yönetim yönetmeye yönelik özellikler içerir. Bu güvenlik özellikleri, güçlü derleme henüz ASP.NET Core uygulamalarının güvenliğini sağlama imkan tanır.

## <a name="aspnet-core-security-features"></a>ASP.NET Core güvenlik özellikleri

Birçok araca ve kitaplığa yerleşik kimlik sağlayıcıları dahil olmak üzere uygulamalarınızın güvenliğini sağlamak için ASP.NET Core sağlar ancak Facebook, Twitter ve LinkedIn gibi 3. taraf kimlik Hizmetleri kullanabilirsiniz. ASP.NET Core ile bir şekilde depolayın ve kod göstermek zorunda kalmadan gizli bilgiler kullanın olan, uygulama gizli anahtarlarının kolayca yönetebilirsiniz.

## <a name="authentication-vs-authorization"></a>Kimlik doğrulaması vs. Yetkilendirme

Kimlik doğrulaması, bir kullanıcı bu bir işletim sistemi, veritabanı, uygulama veya kaynak depolanan karşılaştırılır kimlik bilgilerini sağlayan bir işlemdir. Eşleşirlerse, kullanıcıların kimliğini başarıyla doğrulamak ve, sahip olduğunuz eylemler gerçekleştirebilir bir yetkilendirme sürecinde. Yetkilendirme, bir kullanıcı yapmak için izin verilenden belirleyen işlemini gösterir.

Kimlik doğrulaması düşünmek için başka bir yetkilendirme (sunucu, veritabanı veya uygulama) bu alanı içinde hangi nesneleri için kullanıcının gerçekleştirebileceği eylemleri olsa da sunucu, veritabanı, uygulama veya kaynağa gibi bir alan girin. bir yolu olarak dikkate alınması gereken yoludur.

## <a name="common-vulnerabilities-in-software"></a>Ortak yazılımdaki

ASP.NET Core ve EF yardımcı olan özellikler içeriyor uygulamanızı güvenli hale getirme ve güvenlik ihlallerini önleyin. Aşağıdaki listede yer alan bağlantıları, web uygulamalarında en yaygın güvenlik açıklarından önlemek için yapılan teknikleri belgeleri alır:

* [Siteler arası betik saldırıları](xref:security/cross-site-scripting)
* [SQL ekleme saldırıları](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Siteler arası istek sahteciliği (CSRF)](xref:security/anti-request-forgery)
* [Açık yeniden yönlendirme saldırılarını](xref:security/preventing-open-redirects)

Bilmeniz gereken daha fazla güvenlik açıkları vardır. Daha fazla bilgi için bu belgenin bölümüne bakın *ASP.NET Core güvenlik belgeleri*.

## <a name="aspnet-core-security-documentation"></a>ASP.NET Core güvenlik belgeleri

*   [Kimlik Doğrulaması](xref:security/authentication/index)
    *   [Kimliğe giriş](xref:security/authentication/identity)
    *   [Facebook, Google ve diğer dış sağlayıcıları kullanarak kimlik doğrulamasını etkinleştirme](xref:security/authentication/social/index)
    *   [WS-Federasyon ile kimlik doğrulamasını etkinleştirme](xref:security/authentication/ws-federation)
    * [Windows Kimlik Doğrulaması Yapılandırma](xref:security/authentication/windowsauth)
    *   [Hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm)
    *   [SMS ile iki öğeli kimlik doğrulama](xref:security/authentication/2fa)
    *   [Kimlik olmadan tanımlama bilgisi kimlik doğrulamasını kullan](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Azure AD, bir ASP.NET Core web uygulamasıyla tümleştirme](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Azure AD kullanarak bir WPF uygulamasından ASP.NET Core Web API'si çağırma](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Azure AD kullanarak bir ASP.NET Core web uygulamasında Web API’si çağırma](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Azure AD B2C ile ASP.NET Core web uygulaması](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
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
        *   [DI kullanmayan olmayan senaryolar](xref:security/data-protection/configuration/non-di-scenarios)
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
        *   [Anahtar değiştirilemezliği ve ayarlar](xref:security/data-protection/implementation/key-immutability)
        *   [Anahtar depolama biçimi](xref:security/data-protection/implementation/key-storage-format)
        *   [Kısa ömürlü veri koruma sağlayıcıları](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Uyumluluk](xref:security/data-protection/compatibility/index)
        *   [ASP.NET’te <machineKey> değiştirme](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Kullanıcı verilerinin yetkilendirme tarafından korunduğu bir uygulama oluşturma](xref:security/authorization/secure-data)
*   [Geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets)
*   [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration)
*   [SSL uygulama](xref:security/enforcing-ssl)
*   [İstek Sahteciliğinden Koruma](xref:security/anti-request-forgery)
*   [Açık yeniden yönlendirme saldırılarını önleme](xref:security/preventing-open-redirects)
*   [Siteler Arası Betik kullanmayı önleme](xref:security/cross-site-scripting)
*   [Kaynaklar Arası İstekleri (CORS) etkinleştirme](xref:security/cors)
*   [Uygulamalar arasında tanımlama bilgilerini paylaşma](xref:security/cookie-sharing)
