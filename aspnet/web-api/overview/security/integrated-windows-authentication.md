---
uid: web-api/overview/security/integrated-windows-authentication
title: "Tümleşik Windows kimlik doğrulaması | Microsoft Docs"
author: MikeWasson
description: "ASP.NET Web API'de tümleşik Windows kimlik doğrulaması kullanmayı açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a>Tümleşik Windows kimlik doğrulaması
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Tümleşik Windows kimlik doğrulaması, Kerberos veya NTLM kullanarak kendi Windows kimlik bilgileriyle oturum açmalarını sağlar. İstemci kimlik bilgileri yetkilendirme üst bilgi gönderir. Windows kimlik doğrulaması intranet ortamı için idealdir. Daha fazla bilgi için bkz: [Windows kimlik doğrulaması](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Yararları | Dezavantajlar |
| --- | --- |
| -IIS ile oluşturulmuş. -Kullanıcı kimlik bilgilerinin istekte göndermez. -İstemci bilgisayar (örneğin, intranet uygulama) etki alanına aitse, kullanıcı kimlik bilgilerini girmeniz gerekmez. | -Internet uygulamaları için önerilmez. -İstemci Kerberos veya NTLM desteği gerektirir. -İstemci Active Directory etki alanında olmalıdır. |

> [!NOTE]
> Uygulamanızı Azure üzerinde barındırılan ve bir şirket içi Active Directory etki alanı varsa, şirket içi AD Azure Active Directory ile Federasyon göz önünde bulundurun. Böylece, kullanıcılar kendi şirket içi kimlik bilgileriyle oturum açabilir, ancak kimlik doğrulaması Azure AD tarafından gerçekleştirilir. Daha fazla bilgi için bkz: [Azure kimlik doğrulaması](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Tümleşik Windows kimlik doğrulaması kullanan bir uygulama oluşturmak için MVC 4 Proje Sihirbazı'nda "Intranet uygulaması" şablonunu seçin. Bu proje şablonu Web.config dosyasında aşağıdaki ayar yapar:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

İstemci tarafında, tümleşik Windows kimlik doğrulamasını destekleyen bir tarayıcı ile çalışır [anlaş](http://www.ietf.org/rfc/rfc4559.txt) en önde gelen tarayıcılar içeren kimlik doğrulama düzeni. .NET istemci uygulamaları için **HttpClient** sınıfı, Windows kimlik doğrulamasını destekler:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Windows kimlik doğrulaması için siteler arası istek sahteciliği (CSRF) saldırılarına karşı savunmasızdır. Bkz: [siteler arası istek sahtekarlığı (CSRF) saldırılarını önleme](preventing-cross-site-request-forgery-csrf-attacks.md).
