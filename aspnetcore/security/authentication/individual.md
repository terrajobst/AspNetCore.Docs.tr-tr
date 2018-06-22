---
title: Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleleri
author: rick-anderson
description: Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleleri bulur.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f7fb9e8cd1b5c4cc3283ddd7606a0bbd30f554d5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274433"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleleri

ASP.NET Core kimlik "Tek tek kullanıcı hesaplarını" seçeneği ile Visual Studio Proje şablonları dahil edilir.

Kimlik doğrulama şablonları .NET Core CLI ile kullanılabilir `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Aşağıdaki makaleler bireysel kullanıcı hesapları kullanan ASP.NET Core şablonlarında oluşturulan kodu kullanmayı göstermektedir:

* [SMS ile iki öğeli kimlik doğrulama](xref:security/authentication/2fa)
* [Hesap doğrulama ve ASP.NET Core parola kurtarma](xref:security/authentication/accconfirm)
* [Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma](xref:security/authorization/secure-data)
