---
title: Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core projelerine dayalı makaleler
author: rick-anderson
description: Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core projelerine göre makaleleri bulun.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: 91c5665dc50124b3ba09bdcfbf3ba501f684c604
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463040"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="438a4-103">Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core projelerine dayalı makaleler</span><span class="sxs-lookup"><span data-stu-id="438a4-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="438a4-104">ASP.NET Core kimlik, Visual Studio 'daki proje şablonlarına "bireysel kullanıcı hesapları" seçeneği ile dahildir.</span><span class="sxs-lookup"><span data-stu-id="438a4-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="438a4-105">Kimlik doğrulama şablonları `-au Individual`ile .NET Core CLI kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="438a4-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="438a4-106">Web API kimlik doğrulaması için [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore/issues/5833) bakın.</span><span class="sxs-lookup"><span data-stu-id="438a4-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="438a4-107">Kimlik doğrulaması yok</span><span class="sxs-lookup"><span data-stu-id="438a4-107">No Authentication</span></span>

<span data-ttu-id="438a4-108">Kimlik doğrulaması, .NET Core CLI `-au` seçeneği ile belirtilir.</span><span class="sxs-lookup"><span data-stu-id="438a4-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="438a4-109">Visual Studio 'da **kimlik doğrulaması Değiştir** iletişim kutusu yeni Web uygulamaları için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="438a4-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="438a4-110">Visual Studio 'da yeni Web uygulamaları için varsayılan değer **kimlik doğrulaması**değildir.</span><span class="sxs-lookup"><span data-stu-id="438a4-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="438a4-111">Kimlik doğrulaması olmadan oluşturulan projeler:</span><span class="sxs-lookup"><span data-stu-id="438a4-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="438a4-112">Oturum açmak ve oturumu kapatmak için Web sayfalarını ve Kullanıcı arabirimini bulundurmayın.</span><span class="sxs-lookup"><span data-stu-id="438a4-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="438a4-113">Kimlik doğrulama kodu içermiyor.</span><span class="sxs-lookup"><span data-stu-id="438a4-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="438a4-114">Windows Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="438a4-114">Windows Authentication</span></span>

<span data-ttu-id="438a4-115">`-au Windows` seçeneğiyle .NET Core CLI yeni Web uygulamaları için Windows kimlik doğrulaması belirtilir.</span><span class="sxs-lookup"><span data-stu-id="438a4-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="438a4-116">Visual Studio 'da **kimlik doğrulaması Değiştir** Iletişim kutusu **Windows kimlik doğrulama** seçeneklerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="438a4-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="438a4-117">Windows kimlik doğrulaması seçilirse, uygulama [Windows kimlik doğrulama IIS modülünü](xref:host-and-deploy/iis/modules)kullanacak şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="438a4-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="438a4-118">Windows kimlik doğrulaması, Intranet web sitelerine yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="438a4-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="438a4-119">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="438a4-119">Additional resources</span></span>

<span data-ttu-id="438a4-120">Aşağıdaki makalelerde, bireysel kullanıcı hesapları kullanan ASP.NET Core şablonlarda oluşturulan kodun nasıl kullanılacağı gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="438a4-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="438a4-121">ASP.NET Core hesap onaylama ve parola kurtarma</span><span class="sxs-lookup"><span data-stu-id="438a4-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="438a4-122">Yetkilendirme ile korunan kullanıcı verileriyle ASP.NET Core uygulama oluşturma</span><span class="sxs-lookup"><span data-stu-id="438a4-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
