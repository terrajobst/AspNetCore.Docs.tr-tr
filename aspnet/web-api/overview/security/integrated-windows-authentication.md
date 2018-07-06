---
uid: web-api/overview/security/integrated-windows-authentication
title: Tümleşik Windows kimlik doğrulaması | Microsoft Docs
author: MikeWasson
description: Tümleşik Windows kimlik doğrulaması ASP.NET Web API'sini kullanmayı açıklar.
ms.author: aspnetcontent
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: c15677b7aa66619f1ff32819585340ff497b937d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820412"
---
<a name="integrated-windows-authentication"></a>Tümleşik Windows kimlik doğrulaması
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Tümleşik Windows kimlik doğrulaması, Kerberos veya NTLM kullanarak Windows kimlik bilgileriyle kullanıcıların oturum açmasına sağlar. İstemci kimlik bilgileri yetkilendirme üst bilgisinde gönderir. Windows kimlik doğrulaması intranet ortamı için idealdir. Daha fazla bilgi için [Windows kimlik doğrulaması](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Yararları | Dezavantajlar |
| --- | --- |
| -IIS ile oluşturulmuş. -Kullanıcı kimlik bilgilerinin istekte göndermez. -İstemci bilgisayarı (örneğin, intranet uygulaması) etki alanı dahilse, kullanıcının kimlik bilgilerini girmeniz gerekmez. | -Internet uygulamaları için önerilmez. -İstemci Kerberos veya NTLM desteğini gerektirir. -İstemci, Active Directory etki alanında olmalıdır. |

> [!NOTE]
> Uygulamanızı Azure'da barındırılır ve bir şirket içi Active Directory etki alanı varsa, şirket içi AD'nizi Azure Active Directory ile Federasyon göz önünde bulundurun. Böylece, kullanıcılar kendi şirket içi kimlik bilgileriyle oturum açabilir, ancak kimlik doğrulaması, Azure AD tarafından gerçekleştirilir. Daha fazla bilgi için [Azure kimlik doğrulaması](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Tümleşik Windows kimlik doğrulaması kullanan bir uygulama oluşturmak için MVC 4 Proje Sihirbazı'nda "Intranet uygulaması" şablonu seçin. Bu proje şablonu aşağıdaki ayarları Web.config dosyasına koyar:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

İstemci tarafında, tümleşik Windows kimlik doğrulamasını destekleyen bir tarayıcı ile çalışır. [anlaş](http://www.ietf.org/rfc/rfc4559.txt) çoğu bilinen tarayıcılar içeren kimlik doğrulama düzeni. .NET istemci uygulamaları için **HttpClient** sınıfı, Windows kimlik doğrulamasını destekler:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Windows kimlik doğrulaması için siteler arası istek sahteciliği (CSRF) saldırılarını karşı savunmasızdır. Bkz: [siteler arası istek sahteciliği (CSRF) saldırılarını önleme](preventing-cross-site-request-forgery-csrf-attacks.md).
