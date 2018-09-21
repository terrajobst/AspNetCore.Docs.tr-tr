---
title: Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleler
author: rick-anderson
description: Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleleri keşfedin.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: ac843342ffc73632fbf9f6359c6c1a5878dcef0d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523070"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleler

ASP.NET Core kimliği "Bireysel kullanıcı hesapları" seçeneği ile Visual Studio Proje şablonları dahil edilir.

.NET Core CLI ile kimlik doğrulaması şablonlar kullanılabilir `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<a name="no"></a>
## <a name="no-authentication"></a>Kimlik doğrulama yok

Kimlik doğrulaması ile .NET Core CLI belirtilen `-au` seçeneği. Visual Studio'da **kimlik doğrulamayı Değiştir** iletişim yeni web uygulamaları için kullanılabilir. Visual Studio'da yeni web uygulamaları için varsayılan değer **kimlik doğrulaması yok**.

Kimlik doğrulama olmadan oluşturulan projeleri:

* Web sayfaları ve oturum açın ve oturum kapatma UI içermez.
* Kimlik doğrulama kodu içermez.

<a name="win"></a>
## <a name="windows-authentication"></a>Windows Kimlik Doğrulaması

Windows kimlik doğrulaması için yeni web apps ile .NET Core CLI belirtildiği `-au Windows` seçeneği. Visual Studio'da **kimlik doğrulamayı Değiştir** iletişim kutusu sunar **Windows kimlik doğrulaması** seçenekleri.

Windows kimlik doğrulamasını seçerseniz, uygulamayı kullanmak üzere yapılandırılmış [Windows kimlik doğrulaması IIS Modülü](xref:host-and-deploy/iis/modules). Windows kimlik doğrulaması, Intranet web siteleri için tasarlanmıştır.

## <a name="additional-resources"></a>Ek kaynaklar

Aşağıdaki makaleler, bireysel kullanıcı hesapları kullanan ASP.NET Core şablonları oluşturulan kodu kullanmayı göstermektedir:

* [SMS ile iki öğeli kimlik doğrulama](xref:security/authentication/2fa)
* [Hesap onaylama ve parola kurtarma ASP.NET Core](xref:security/authentication/accconfirm)
* [Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma](xref:security/authorization/secure-data)
