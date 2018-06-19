---
uid: web-api/overview/security/forms-authentication
title: Form kimlik doğrulamasını ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API'de form kimlik doğrulaması kullanmayı açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566799"
---
<a name="forms-authentication-in-aspnet-web-api"></a>ASP.NET Web API form kimlik doğrulaması
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Form kimlik doğrulaması, kullanıcının kimlik bilgilerini sunucuya göndermek için bir HTML formuna kullanır. Bir Internet standardıdır değil. Böylece kullanıcı HTML form ile etkileşim kurabilen form kimlik doğrulaması yalnızca web bir web uygulamasından adlı API'leri için uygundur.

| Yararları | Dezavantajlar |
| --- | --- |
| -Uygulamak kolay: ASP.NET oluşturulmuş. -Kullanıcı hesaplarını yönetme kolaylaştırır ASP.NET üyelik sağlayıcısı kullanır. | -Olmayan bir standart HTTP kimlik doğrulama mekanizması; Standart Authorization Üstbilgisi yerine HTTP tanımlama bilgilerini kullanır. -Bir tarayıcı istemcisi gerektirir. -Kimlik bilgileri düz metin olarak gönderilir. -Karşı savunmasız siteler arası istek sahtekarlığı (CSRF); Anti-CSRF ölçüleri gerektirir. -Nonbrowser istemcilerden kullanmak zordur. Oturum açma bir tarayıcı gerektirir. -Kullanıcı kimlik bilgileri isteği gönderilir. -Bazı kullanıcılar tanımlama bilgilerini devre dışı bırakın. |

Kısaca, ASP.NET forms kimlik doğrulaması gibi çalışır:

1. İstemci kimlik doğrulaması gerektiren bir kaynak ister.
2. Kullanıcının kimliği doğrulanmazsa, sunucu HTTP 302 (bulundu) verir ve bir oturum açma sayfasına yönlendirir.
3. Kullanıcı kimlik bilgilerini girer ve form gönderir.
4. Sunucunun özgün URI'nin yönlendiren başka bir HTTP 302 döndürür. Bu yanıt bir kimlik doğrulama tanımlama bilgisi içeriyor.
5. İstemci yeniden kaynak ister. Sunucu, isteği verir. böylece istek kimlik doğrulama tanımlama içerir.

![](forms-authentication/_static/image1.png)

Daha fazla bilgi için bkz: [form kimlik doğrulaması bir genel bakış.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Form kimlik doğrulaması Web API ile kullanma

Form kimlik doğrulaması kullanan bir uygulama oluşturmak için MVC 4 Proje Sihirbazı'nda "Internet uygulaması" şablonunu seçin. Bu şablon, MVC denetleyicileri hesap yönetimi için oluşturur. ASP.NET sonbaharda 2012 Update kullanılabilir "Tek sayfa uygulaması" şablonu de kullanabilirsiniz.

Web API denetleyicilerinizi kullanarak erişimi kısıtlayabilirsiniz `[Authorize]` açıklandığı gibi öznitelik [[Authorize] özniteliğini kullanarak](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Form kimlik doğrulaması isteklerinin kimlik doğrulaması için bir oturum tanımlama bilgisi kullanır. Tarayıcılar, hedef web sitesine tüm ilgili tanımlama bilgilerini otomatik olarak gönder. Bu özellik form kimlik doğrulaması siteler arası istek sahtekarlığı (CSRF) saldırılarını bakın için olası saldırılara açık hale getirir [önleme siteler arası istek sahteciliği (CSRF) saldırılarını](preventing-cross-site-request-forgery-csrf-attacks.md).

Form kimlik doğrulaması kullanıcının kimlik bilgilerini şifrelemez. Bu nedenle, form kimlik doğrulaması ile SSL kullanılmadığı sürece güvenli değildir. Bkz: [Web API'de SSL ile çalışma](working-with-ssl-in-web-api.md).
