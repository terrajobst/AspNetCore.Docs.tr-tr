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
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9d5610eb61088a8e7573ba6399c771f0957ff437
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395337"
---
<a name="basic-authentication-in-aspnet-web-api"></a>ASP.NET Web API'de temel kimlik doğrulaması
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Temel kimlik doğrulaması tanımlanmış [RFC 2617, HTTP kimlik doğrulaması: temel ve Özet erişimi kimlik doğrulaması](http://www.ietf.org/rfc/rfc2617.txt).

Dezavantajlar

- Kullanıcı kimlik bilgileri isteği gönderilir.
- Kimlik bilgileri düz metin olarak gönderilir.
- Kimlik bilgileri, her bir istekle gönderilir.
- Yolu, oturum dışındaki tarayıcı oturumu sonlandırarak.
- Siteler arası istek sahteciliği (CSRF); karşı savunmasız Anti-CSRF ölçülerin gerektirir.

Yararları

- Internet standart.
- Tüm bilinen tarayıcılar tarafından desteklenir.
- Görece basit protokolü.

Temel kimlik doğrulaması gibi çalışır:

1. Sunucu, isteği kimlik doğrulaması gerektiriyorsa, 401 (yetkisiz) döndürür. Yanıt, sunucunun temel kimlik doğrulamasını destekleyip belirten bir WWW-Authenticate üstbilgisi içeriyor.
2. İstemci, yetkilendirme üst bilgisinde istemci kimlik bilgileri ile başka bir istek gönderir. Kimlik bilgileri "adı: parola", base64 ile kodlanmış dize olarak biçimlendirilir. Kimlik bilgileri şifrelenmez.

Temel kimlik doğrulaması, bir "realm." bağlamında yapılır Sunucu, WWW-Authenticate üstbilgisi bölge adını içerir. Kullanıcının kimlik bilgilerini bu bölge içinde geçerlidir. Bir bölge tam kapsamı, sunucu tarafından tanımlanır. Örneğin sırada bölüm kaynakları için çeşitli bölgeleri tanımlayabilir.

![](basic-authentication/_static/image1.png)

Kimlik bilgileri şifrelenmemiş gönderilir, temel kimlik doğrulaması yalnızca HTTPS üzerinden güvenli olmasıdır. Bkz: [Web API'de SSL ile çalışma](working-with-ssl-in-web-api.md).

Temel kimlik doğrulaması da CSRF saldırılarına karşı savunmasız durumdadır. Kullanıcı kimlik bilgilerini girdikten sonra tarayıcının otomatik olarak bunları sonraki isteklerde authenticateasync aynı etki alanına oturum süresi boyunca gönderir. Bu AJAX istekleri içerir. Bkz: [siteler arası istek sahteciliği (CSRF) saldırılarını önleme](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>IIS ile temel kimlik doğrulaması

IIS temel kimlik doğrulamasını destekler, ancak bir uyarı yok: kullanıcı, Windows kimlik bilgilerine karşı kimlik doğrulaması. Kullanıcı, sunucunun etki alanında bir hesabınızın olması gerekir anlamına gelir. Genel kullanıma yönelik bir web sitesi için genellikle bir ASP.NET üyelik sağlayıcı karşı kimlik doğrulaması istersiniz.

IIS kullanarak temel kimlik doğrulamasını etkinleştirmek için kimlik doğrulama modu, ASP.NET projesinin Web.config dosyasındaki "Windows" ayarlayın:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

Bu modda, IIS kimlik doğrulaması için Windows kimlik bilgilerini kullanır. Ayrıca, IIS temel kimlik doğrulamasını etkinleştirmeniz gerekir. IIS Yöneticisi'nde özellikler görünümüne gidin, kimlik doğrulama yöntemini seçin ve temel kimlik doğrulamasını etkinleştirin.

![](basic-authentication/_static/image2.png)

Web API projesinde, ekleyin `[Authorize]` kimlik doğrulaması gerektiren herhangi bir denetleyici eylemleri için özniteliği.

Bir istemci kendi yetkilendirme üst bilgisi istekte ayarlayarak kimliğini doğrular. Tarayıcı istemcilerinin, otomatik olarak bu adımı gerçekleştirin. Nonbrowser istemcileri üstbilgi ayarlamanız gerekir.

## <a name="basic-authentication-with-custom-membership"></a>Özel üyelik ile temel kimlik doğrulaması

Belirtildiği gibi temel IIS ile yerleşik kimlik doğrulaması Windows kimlik bilgilerini kullanır. Barındırma sunucusundaki kullanıcılarınız için hesapları oluşturmak için ihtiyacınız anlamına gelir. Ancak bir Internet uygulaması için kullanıcı hesapları genellikle bir dış veritabanında depolanır.

Temel kimlik doğrulaması gerçekleştiren bir HTTP modülü nasıl aşağıdaki kod. Bir ASP.NET üyelik sağlayıcısı değiştirerek kolayca takılabilir `CheckPassword` Bu örnekte işlevsiz bir yöntemi olan yöntem.

Web API 2'de yazma dikkate almanız gereken bir [kimlik doğrulaması filtresini](authentication-filters.md) veya [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/index.md), yerine bir HTTP modülü.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

HTTP modülü etkinleştirmek için web.config dosyanızda şunları ekleyin **system.webServer** bölümü:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

"YourAssemblyName" ("dll" uzantısı dahil değil) derlemenin adı ile değiştirin.

Forms veya Windows kimlik doğrulaması gibi diğer kimlik doğrulama düzenleri devre dışı bırakmanız gerekir
