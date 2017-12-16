---
title: "ASP.NET Core güvenliğine genel bakış | Microsoft Docs"
author: rachelappel
description: "ASP.NET Core kimlik doğrulama, yetkilendirme ve güvenlik temel kavramları hakkında bilgi edinin"
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: 3f4df08d6cf5d183735ae4b4ec4f07ed60a9623a
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="aspnet-core-security-overview"></a>ASP.NET Core güvenliğine genel bakış

ASP.NET Core geliştiricilerin kolayca yapılandırmanıza ve uygulamalarını güvenliğini yönetmenize olanak sağlar. ASP.NET Core kimlik doğrulama, yetkilendirme, veri koruma, SSL zorlama, uygulama parolaları, koruma istek sahteciliğine koruma ve CORS yönetim yönetmeye yönelik özellikler içerir. Bu güvenlik özellikleri, sağlam yapı henüz ASP.NET Core uygulamaları güvenli olanak tanır. 

## <a name="aspnet-core-security-features"></a>ASP.NET Core güvenlik özellikleri

ASP.NET Core birçok araçları ve yerleşik kimlik sağlayıcıları gibi uygulamalarınızın güvenliğini sağlamak için kitaplıkları sağlar ancak 3 taraf kimlik hizmetlerini Facebook, Twitter ve LinkedIn gibi kullanabilirsiniz. ASP.NET Core ile depolamak ve kodda kullanıma gerek kalmadan gizli bilgileri kullanmak için bir yoldur uygulama sırrı kolayca yönetebilirsiniz. 

## <a name="authentication-vs-authorization"></a>Kimlik doğrulama vs. Yetkilendirme

Kimlik doğrulaması, bir kullanıcı daha sonra bu bir işletim sistemi, veritabanı, uygulama veya kaynak depolanan karşılaştırılır kimlik bilgilerini sağlayan bir işlemdir. Eşleşirlerse, kullanıcıların kimliğini başarıyla doğrulamak ve, yetkili eylemler gerçekleştirebilir bir yetkilendirme sürecinde. Yetkilendirme kullanıcı yapmak için verileceğini belirler işleme başvuruyor. 

Kimliğini düşünmeye başka bir kullanıcı (sunucu, veritabanı veya uygulamanın) alan içinde hangi nesneleri için hangi eylemleri olsa da yetkilendirme sunucusu, veritabanı, uygulama veya kaynak gibi bir alanı girmek için bir yol olarak dikkate alınması gereken yoludur.

## <a name="common-vulnerabilities-in-software"></a>Ortak Güvenlik Açıkları yazılım

ASP.NET Çekirdeği ve EF yardımcı özellikler içeriyor uygulamalarınızın güvenliğini sağlamak ve güvenlik açıklarına. Aşağıdaki listesi bağlantılar, web uygulamalarında en yaygın güvenlik açıkları önlemek için belgeleri ayrıntılandıran teknikleri alır:

* [Site komut dosyası saldırıları arası](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [SQL ekleme saldırıları](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Siteler arası istek sahtekarlığı (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Açık yeniden yönlendirme saldırılarına](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Bilmeniz gereken daha fazla güvenlik açıkları vardır. Daha fazla bilgi için bu belgedeki bölümüne bakarak *ASP.NET güvenlik belgelerine*. 

## <a name="aspnet-security-documentation"></a>ASP.NET güvenlik belgeleri

*   [Kimlik doğrulaması](authentication/index.md)
    *   [Kimlik giriş](authentication/identity.md)
    *   [Facebook, Google ve diğer dış sağlayıcılarını kullanarak kimlik doğrulamasını etkinleştirme](authentication/social/index.md)
    * [Windows kimlik doğrulamasını yapılandırma](authentication/windowsauth.md)
    *   [Hesap onaylamayı ve parola kurtarma](authentication/accconfirm.md)
    *   [SMS ile iki öğeli kimlik doğrulama](authentication/2fa.md) 
    *   [ASP.NET Core kimlik olmadan tanımlama bilgisi kimlik doğrulamasını kullanma](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Bir ASP.NET Core Web uygulamasına Azure AD tümleştirme](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Azure AD kullanarak bir WPF uygulamasında ASP.NET Core Web API'si çağırma](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Azure AD kullanarak bir ASP.NET Core Web uygulamasında Web API'si çağırma](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Azure AD B2C ile bir ASP.NET Core web uygulama](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [ASP.NET Core uygulamaları IdentityServer4 ile güvenli hale getirme](https://identityserver4.readthedocs.io)
*   [Yetkilendirme](authorization/index.md)
    *   [Giriş](authorization/introduction.md)
    *   [Yetkilendirme tarafından korunan kullanıcı verileri ile uygulama oluşturma](xref:security/authorization/secure-data)
    *   [Basit yetkilendirme](authorization/simple.md)
    *   [Rol tabanlı yetkilendirme](authorization/roles.md)
    *   [Talep tabanlı yetkilendirme](authorization/claims.md)
    *   [Özel ilke tabanlı yetkilendirme](authorization/policies.md)
    *   [Gereksinim işleyiciler bağımlılık ekleme](authorization/dependencyinjection.md)
    *   [Kaynak tabanlı bir yetkilendirme](authorization/resourcebased.md)
    *   [Görünüm tabanlı bir yetkilendirme](authorization/views.md)
    *   [Kimlik düzeni tarafından sınırlama](authorization/limitingidentitybyscheme.md)
*   [Veri koruma](data-protection/index.md)
    *   [Veri koruma giriş](data-protection/introduction.md)
    *   [Veri Koruma API'ları ile çalışmaya başlama](data-protection/using-data-protection.md)
    *   [Tüketici API'leri](data-protection/consumer-apis/index.md)
        *   [Tüketici API'leri genel bakış](data-protection/consumer-apis/overview.md)
        *   [Amaç dizeleri](data-protection/consumer-apis/purpose-strings.md)
        *   [Amaç hiyerarşi ve çoklu kiracı](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Parola karma](data-protection/consumer-apis/password-hashing.md)
        *   [Korumalı yüklerini ömrü sınırlama](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Kaldırmayı yükü, anahtarları iptal edildi](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Yapılandırma](data-protection/configuration/index.md)
        *   [Veri korumasını yapılandırma](data-protection/configuration/overview.md)
        *   [Varsayılan ayarları](data-protection/configuration/default-settings.md)
        *   [Makine geniş İlkesi](data-protection/configuration/machine-wide-policy.md)
        *   [Olmayan dı kullanan senaryolar](data-protection/configuration/non-di-scenarios.md)
    *   [Genişletilebilirlik API](data-protection/extensibility/index.md)
        *   [Çekirdek şifreleme genişletilebilirliği](data-protection/extensibility/core-crypto.md)
        *   [Anahtar Yönetimi genişletilebilirliği](data-protection/extensibility/key-management.md)
        *   [Çeşitli API'ler](data-protection/extensibility/misc-apis.md)
    *   [Uygulama](data-protection/implementation/index.md)
        *   [Kimliği doğrulanmış şifreleme ayrıntıları](data-protection/implementation/authenticated-encryption-details.md)
        *   [Alt anahtar türetme ve kimliği doğrulanmış şifreleme](data-protection/implementation/subkeyderivation.md)
        *   [İçerik üstbilgileri](data-protection/implementation/context-headers.md)
        *   [Anahtar Yönetimi](data-protection/implementation/key-management.md)
        *   [Anahtar depolama sağlayıcıları](data-protection/implementation/key-storage-providers.md)
        *   [REST anahtar şifrelemesi](data-protection/implementation/key-encryption-at-rest.md)
        *   [Anahtar girişi ve ayarlarını değiştirme](data-protection/implementation/key-immutability.md)
        *   [Anahtar depolama biçimi](data-protection/implementation/key-storage-format.md)
        *   [Kısa ömürlü veri koruma sağlayıcısı](data-protection/implementation/key-storage-ephemeral.md)
    *   [Uyumluluk](data-protection/compatibility/index.md)
        *   [Tanımlama bilgilerini uygulamalar arasında paylaşma](data-protection/compatibility/cookie-sharing.md)
        *   [Değiştirme <machineKey> ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Yetkilendirme tarafından korunan kullanıcı verileri ile uygulama oluşturma](xref:security/authorization/secure-data)
*   [Geliştirme sırasında uygulama sırrı güvenli depolama](app-secrets.md)
*   [Azure anahtar kasası yapılandırma sağlayıcısı](key-vault-configuration.md)
*   [SSL zorlama](enforcing-ssl.md)
*   [Koruma istek Sahteciliğine](anti-request-forgery.md)
*   [Açık yeniden yönlendirme saldırılarını önleme](preventing-open-redirects.md)
*   [Siteler arası komut dosyası önleme](cross-site-scripting.md)
*   [Cross-Origin istekleri (CORS) etkinleştirme](cors.md)
