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
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="3f1da-103">Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core projeleri göre makaleler</span><span class="sxs-lookup"><span data-stu-id="3f1da-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="3f1da-104">ASP.NET Core kimliği "Bireysel kullanıcı hesapları" seçeneği ile Visual Studio Proje şablonları dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="3f1da-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="3f1da-105">.NET Core CLI ile kimlik doğrulaması şablonlar kullanılabilir `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="3f1da-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

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
## <a name="no-authentication"></a><span data-ttu-id="3f1da-106">Kimlik doğrulama yok</span><span class="sxs-lookup"><span data-stu-id="3f1da-106">No Authentication</span></span>

<span data-ttu-id="3f1da-107">Kimlik doğrulaması ile .NET Core CLI belirtilen `-au` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="3f1da-107">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="3f1da-108">Visual Studio'da **kimlik doğrulamayı Değiştir** iletişim yeni web uygulamaları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3f1da-108">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="3f1da-109">Visual Studio'da yeni web uygulamaları için varsayılan değer **kimlik doğrulaması yok**.</span><span class="sxs-lookup"><span data-stu-id="3f1da-109">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="3f1da-110">Kimlik doğrulama olmadan oluşturulan projeleri:</span><span class="sxs-lookup"><span data-stu-id="3f1da-110">Projects created with no authentication:</span></span>

* <span data-ttu-id="3f1da-111">Web sayfaları ve oturum açın ve oturum kapatma UI içermez.</span><span class="sxs-lookup"><span data-stu-id="3f1da-111">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="3f1da-112">Kimlik doğrulama kodu içermez.</span><span class="sxs-lookup"><span data-stu-id="3f1da-112">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="3f1da-113">Windows Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="3f1da-113">Windows Authentication</span></span>

<span data-ttu-id="3f1da-114">Windows kimlik doğrulaması için yeni web apps ile .NET Core CLI belirtildiği `-au Windows` seçeneği.</span><span class="sxs-lookup"><span data-stu-id="3f1da-114">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="3f1da-115">Visual Studio'da **kimlik doğrulamayı Değiştir** iletişim kutusu sunar **Windows kimlik doğrulaması** seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="3f1da-115">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="3f1da-116">Windows kimlik doğrulamasını seçerseniz, uygulamayı kullanmak üzere yapılandırılmış [Windows kimlik doğrulaması IIS Modülü](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="3f1da-116">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="3f1da-117">Windows kimlik doğrulaması, Intranet web siteleri için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3f1da-117">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f1da-118">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3f1da-118">Additional resources</span></span>

<span data-ttu-id="3f1da-119">Aşağıdaki makaleler, bireysel kullanıcı hesapları kullanan ASP.NET Core şablonları oluşturulan kodu kullanmayı göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="3f1da-119">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="3f1da-120">SMS ile iki öğeli kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="3f1da-120">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="3f1da-121">Hesap onaylama ve parola kurtarma ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f1da-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="3f1da-122">Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="3f1da-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
