---
title: "Bireysel kullanıcı hesapları ile oluşturulmuş projelerde tabanlı makaleler"
author: rick-anderson
description: "Bu belge bireysel kullanıcı hesapları ile oluşturulan projeleri göre makaleleri listeler."
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: aee18fa08fbc5c8452ca2b401d32858edaf55e7c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
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
