---
uid: web-api/overview/security/forms-authentication
title: ASP.NET Web API'de Forms kimlik doğrulaması | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API'de Forms kimlik doğrulaması kullanmayı açıklar.
ms.author: aspnetcontent
ms.date: 12/12/2012
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b73adf1390ce9573cd2979010932365349caea0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827540"
---
<a name="forms-authentication-in-aspnet-web-api"></a>ASP.NET Web API'de Forms kimlik doğrulaması
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Form kimlik doğrulaması, kullanıcının kimlik bilgilerini sunucuya göndermek için bir HTML formuna kullanır. Bir Internet standardıdır değil. Kullanıcı HTML formla etkileşim kurabilir, form kimlik doğrulaması yalnızca web bir web uygulamasından adlı API için uygundur.

| Yararları | Dezavantajlar |
| --- | --- |
| -Uygulanması kolaydır: ASP.NET ile oluşturulmuş. -Kullanıcı hesaplarını yönetme kolaylaştırır ASP.NET üyelik sağlayıcısı kullanır. | -Bir standart HTTP kimlik doğrulaması mekanizmadan değil; Standart yetkilendirme üst bilgisi yerine HTTP tanımlama bilgileri kullanır. -Bir tarayıcı istemcisi gerektirir. -Kimlik bilgileri düz metin olarak gönderilir. -Savunmasız siteler arası istek sahteciliği (CSRF); Anti-CSRF ölçülerin gerektirir. -Nonbrowser istemcilerden kullanmak zordur. Oturum açma, bir tarayıcı gerektirir. -Kullanıcı kimlik bilgileri isteği gönderilir. -Bazı kullanıcılar tanımlama bilgilerini devre dışı bırakın. |

Kısaca, ASP.NET formları kimlik doğrulaması şöyle çalışır:

1. İstemci kimlik doğrulaması gerektiren bir kaynak ister.
2. Kullanıcının kimliği doğrulanmazsa, HTTP 302 (bulunamadı) döndürür ve sunucu bir oturum açma sayfasına yönlendirir.
3. Kullanıcı kimlik bilgilerini girer ve formu gönderir.
4. Sunucunun özgün URI'nin yönlendiren başka bir HTTP 302 döndürür. Bu yanıt, bir kimlik doğrulama tanımlama bilgisi içerir.
5. İstemci, kaynak yeniden ister. Sunucu, istek verir. Bu nedenle istek kimlik doğrulama tanımlama bilgisi içerir.

![](forms-authentication/_static/image1.png)

Daha fazla bilgi için [form kimlik doğrulaması bir genel bakış.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Form kimlik doğrulaması ile Web API kullanma

Form kimlik doğrulaması kullanan bir uygulama oluşturmak için MVC 4 Proje Sihirbazı'nda "Internet uygulaması" şablonu seçin. Bu şablon, hesap yönetimi için MVC denetleyicisi oluşturur. ASP.NET Fall 2012 güncelleştirmede kullanılabilir "Tek sayfalı uygulama" şablonu da kullanabilirsiniz.

Web API denetleyicilerinizi kullanarak erişimi kısıtlayabilir `[Authorize]` açıklandığı gibi öznitelik [[Authorize] özniteliği kullanılarak](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Form kimlik doğrulaması isteklerinin kimliğini doğrulamak için bir oturum tanımlama bilgisini kullanır. Tarayıcılar, hedef web sitesine ilgili tüm tanımlama bilgilerini otomatik olarak gönder. Bu özellik, form kimlik doğrulaması için siteler arası istek sahteciliği (CSRF) saldırılarını bakın savunmasız kılar [önleme siteler arası istek sahteciliği (CSRF) saldırılarını](preventing-cross-site-request-forgery-csrf-attacks.md).

Form kimlik doğrulaması, kullanıcının kimlik bilgilerini şifrelemez. Bu nedenle, form kimlik doğrulaması ile SSL kullanılmadığı sürece güvenli değildir. Bkz: [Web API'de SSL ile çalışma](working-with-ssl-in-web-api.md).
