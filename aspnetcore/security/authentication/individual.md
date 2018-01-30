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
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="f1cd9-103">Bireysel kullanıcı hesapları ile oluşturulmuş projelerde tabanlı makaleler</span><span class="sxs-lookup"><span data-stu-id="f1cd9-103">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="f1cd9-104">ASP.NET Core kimlik "Tek tek kullanıcı hesaplarını" seçeneği ile Visual Studio Proje şablonları dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="f1cd9-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="f1cd9-105">Kimlik doğrulama şablonları .NET Core CLI ile kullanılabilir `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="f1cd9-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="f1cd9-106">Aşağıdaki makaleler bireysel kullanıcı hesapları kullanan ASP.NET Core şablonlarında oluşturulan kodu kullanmayı göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="f1cd9-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="f1cd9-107">SMS ile iki öğeli kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="f1cd9-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="f1cd9-108">Hesap doğrulama ve ASP.NET Core parola kurtarma</span><span class="sxs-lookup"><span data-stu-id="f1cd9-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="f1cd9-109">Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="f1cd9-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
