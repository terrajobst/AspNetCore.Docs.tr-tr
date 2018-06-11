---
title: Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleleri
author: rick-anderson
description: Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleleri bulur.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 6d05cd8c0ee6c9029eb9bbafc15d9b19ee7633de
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252028"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="b6735-103">Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleleri</span><span class="sxs-lookup"><span data-stu-id="b6735-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="b6735-104">ASP.NET Core kimlik "Tek tek kullanıcı hesaplarını" seçeneği ile Visual Studio Proje şablonları dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="b6735-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="b6735-105">Kimlik doğrulama şablonları .NET Core CLI ile kullanılabilir `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="b6735-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

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

<span data-ttu-id="b6735-107">Aşağıdaki makaleler bireysel kullanıcı hesapları kullanan ASP.NET Core şablonlarında oluşturulan kodu kullanmayı göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="b6735-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="b6735-108">SMS ile iki öğeli kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="b6735-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="b6735-109">Hesap doğrulama ve ASP.NET Core parola kurtarma</span><span class="sxs-lookup"><span data-stu-id="b6735-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b6735-110">Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="b6735-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
