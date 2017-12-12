---
title: "Bireysel kullanıcı hesapları ile oluşturulmuş projelerde tabanlı makaleler"
author: rick-anderson
description: "Bu belge bireysel kullanıcı hesapları ile oluşturulan projeleri göre makaleleri listeler."
keywords: ASP.NET Core, yetkilendirme, IAuthorizationService
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>Bireysel kullanıcı hesapları ile oluşturulmuş projelerde tabanlı makaleler

ASP.NET Core kimlik "Tek tek kullanıcı hesaplarını" seçeneği ile Visual Studio Proje şablonları dahil edilir.

Kimlik doğrulama şablonları .NET Core CLI ile kullanılabilir `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

Aşağıdaki makaleler bireysel kullanıcı hesapları kullanan ASP.NET Core şablonlarında oluşturulan kodu kullanmayı göstermektedir:

* [SMS ile iki öğeli kimlik doğrulama](xref:security/authentication/2fa)
* [Hesap doğrulama ve ASP.NET Core parola kurtarma](xref:security/authentication/accconfirm)
* [Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma](xref:security/authorization/secure-data)