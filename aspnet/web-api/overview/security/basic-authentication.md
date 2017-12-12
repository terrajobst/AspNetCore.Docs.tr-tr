---
uid: web-api/overview/security/basic-authentication
title: "ASP.NET Web API'de temel kimlik doğrulaması | Microsoft Docs"
author: MikeWasson
description: "ASP.NET Web API'de temel kimlik doğrulaması kullanmayı açıklar."
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
---
<a name="basic-authentication-in-aspnet-web-api"></a>ASP.NET Web API'de temel kimlik doğrulaması
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Temel kimlik doğrulaması tanımlanmış [RFC 2617, HTTP kimlik doğrulaması: temel ve Özet erişimi kimlik doğrulaması](http://www.ietf.org/rfc/rfc2617.txt).

Dezavantajlar

- Kullanıcı kimlik bilgileri isteği gönderilir.
- Kimlik bilgileri düz metin olarak gönderilir.
- Kimlik bilgileri sahip her isteği gönderilir.
- Yolu, oturum dışındaki Tarayıcı oturumunu sona erdirme.
- Karşı savunmasız siteler arası istek sahtekarlığı (CSRF); Anti-CSRF ölçüleri gerektirir.

Yararları

- Internet standardı.
- Önde gelen tüm tarayıcılar tarafından desteklenir.
- Görece basit protokolü.

Temel kimlik doğrulaması gibi çalışır:

1. Bir isteği kimlik doğrulaması gerektiriyorsa, sunucunun 401 (yetkisiz) döndürür. Yanıt, sunucunun temel kimlik doğrulamasını destekleyen belirten bir WWW-Authenticate üstbilgisi içeriyor.
2. İstemci yetkilendirme üst istemci kimlik bilgilerine sahip başka bir istek gönderir. Kimlik bilgileri "name: parola", base64 ile kodlanmış dize olarak biçimlendirilir. Kimlik bilgileri şifrelenmez.

Temel kimlik doğrulaması "bölge." bağlamında gerçekleştirilir Sunucunun bölgesi için ad WWW-Authenticate üst bilgi içerir. Kullanıcının kimlik bilgileri, o bölge içinde geçerlidir. Bir bölge tam kapsamı sunucu tarafından tanımlanır. Örneğin, bölüm kaynaklara sırayla birkaç bölgeleri tanımlayabilirsiniz.

![](basic-authentication/_static/image1.png)

Kimlik bilgileri şifrelenmemiş gönderildiğinden, temel kimlik doğrulaması yalnızca HTTPS üzerinden güvenlidir. Bkz: [Web API'de SSL ile çalışma](working-with-ssl-in-web-api.md).

Temel kimlik doğrulaması da CSRF saldırılarına açıktır. Kullanıcı kimlik bilgilerini girdikten sonra tarayıcı otomatik olarak bunları sonraki isteklerde aynı etki alanına oturum süresince gönderir. AJAX istekleri içerir. Bkz: [siteler arası istek sahtekarlığı (CSRF) saldırılarını önleme](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>IIS ile temel kimlik doğrulaması

IIS temel kimlik doğrulamasını destekler, ancak bir uyarı yok: kullanıcı için Windows kimlik bilgilerini karşı kimlik doğrulaması. Kullanıcı hesabınız sunucunun etki alanına sahip olmalıdır anlamına gelir. Genel kullanıma yönelik web sitesi için genellikle bir ASP.NET üyelik sağlayıcısı karşı kimlik doğrulaması istiyor.

IIS kullanarak temel kimlik doğrulamasını etkinleştirmek için ASP.NET projesi Web.config dosyasında "Windows" için kimlik doğrulama modunu ayarlayın:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

Bu modda, IIS kimlik doğrulaması için Windows kimlik bilgilerini kullanır. Ayrıca, IIS'de temel kimlik doğrulamasını etkinleştirmeniz gerekir. IIS Yöneticisi ' nde özellikler görünümüne gidin, kimlik doğrulama yöntemini seçin ve temel kimlik doğrulamasını etkinleştirin.

![](basic-authentication/_static/image2.png)

Web API projenize eklemek `[Authorize]` kimlik doğrulaması gerektiren herhangi bir denetleyici eylemleri özniteliği.

Bir istemci kendi istekte Authorization Üstbilgisi ayarlayarak kimliğini doğrular. Tarayıcı istemcileri otomatik olarak bu adımı gerçekleştirin. Nonbrowser istemcilerini üstbilgi ayarlamanız gerekir.

## <a name="basic-authentication-with-custom-membership"></a>Özel üyeliği ile temel kimlik doğrulaması

Belirtildiği gibi temel IIS içinde yerleşik olarak kimlik doğrulaması Windows kimlik bilgilerini kullanır. Barındırma sunucusundaki kullanıcılarınız için hesapları oluşturmanıza gerek anlamına gelir. Ancak bir internet uygulama için kullanıcı hesapları genellikle bir dış veritabanında depolanır.

Temel kimlik doğrulaması gerçekleştiren bir HTTP modülü nasıl aşağıdaki kod. Bir ASP.NET üyelik sağlayıcısı değiştirerek kolayca ekleyebilirsiniz `CheckPassword` Bu örnekte kukla bir yöntemdir yöntemi.

Web API 2'de yazma dikkate almanız gereken bir [kimlik doğrulaması filtresini](authentication-filters.md) veya [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/index.md), HTTP modülü yerine.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

HTTP modülü etkinleştirmek için web.config dosyanızda şunları ekleyin **system.webServer** bölümü:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

"YourAssemblyName" ("dll" uzantısı dahil değil) derleme adı ile değiştirin.

Formlar veya Windows kimlik doğrulaması gibi diğer kimlik doğrulama şemasını devre dışı bırakmanız gerekir
