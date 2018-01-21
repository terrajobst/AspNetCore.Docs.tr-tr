---
title: "Bireysel kullanıcı hesapları ile oluşturulmuş projelerde tabanlı makaleler"
author: rick-anderson
description: "Bu belge bireysel kullanıcı hesapları ile oluşturulan projeleri göre makaleleri listeler."
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 844514f2970b761ec65589765eb21421cd1962a1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="ba0de-103">Bireysel kullanıcı hesapları ile oluşturulmuş projelerde tabanlı makaleler</span><span class="sxs-lookup"><span data-stu-id="ba0de-103">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="ba0de-104">ASP.NET Core kimlik "Tek tek kullanıcı hesaplarını" seçeneği ile Visual Studio Proje şablonları dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="ba0de-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="ba0de-105">Kimlik doğrulama şablonları .NET Core CLI ile kullanılabilir `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="ba0de-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="ba0de-106">Aşağıdaki makaleler bireysel kullanıcı hesapları kullanan ASP.NET Core şablonlarında oluşturulan kodu kullanmayı göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="ba0de-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="ba0de-107">SMS ile iki öğeli kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="ba0de-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="ba0de-108">Hesap doğrulama ve ASP.NET Core parola kurtarma</span><span class="sxs-lookup"><span data-stu-id="ba0de-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ba0de-109">Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ba0de-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
