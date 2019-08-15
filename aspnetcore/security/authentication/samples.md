---
title: ASP.NET Core için kimlik doğrulama örnekleri
author: rick-anderson
description: ASP.NET Core deposundaki kimlik doğrulama örneklerine bağlantılar sağlar.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: efa177245faceddad4eb80de9e6f6d38e1a4261c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022417"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="68cb1-103">ASP.NET Core için kimlik doğrulama örnekleri</span><span class="sxs-lookup"><span data-stu-id="68cb1-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="68cb1-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="68cb1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="68cb1-105">[ASP.NET Core deposu](https://github.com/aspnet/AspNetCore) , *aspnetcore/src/Security/Samples* klasöründe aşağıdaki kimlik doğrulama örneklerini içerir:</span><span class="sxs-lookup"><span data-stu-id="68cb1-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="68cb1-106">Talep dönüştürme</span><span class="sxs-lookup"><span data-stu-id="68cb1-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="68cb1-107">Tanımlama bilgisi kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="68cb1-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="68cb1-108">Özel ilke sağlayıcısı-ıauthorizationpolicyprovider</span><span class="sxs-lookup"><span data-stu-id="68cb1-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="68cb1-109">Dinamik kimlik doğrulama şemaları ve seçenekleri</span><span class="sxs-lookup"><span data-stu-id="68cb1-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="68cb1-110">Dış talepler</span><span class="sxs-lookup"><span data-stu-id="68cb1-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="68cb1-111">İsteğe bağlı tanımlama bilgisi ve başka bir kimlik doğrulama düzeni arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="68cb1-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="68cb1-112">Statik dosyalara erişimi kısıtlar</span><span class="sxs-lookup"><span data-stu-id="68cb1-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="68cb1-113">Örnekleri çalıştırın</span><span class="sxs-lookup"><span data-stu-id="68cb1-113">Run the samples</span></span>

* <span data-ttu-id="68cb1-114">Bir [dal](https://github.com/aspnet/AspNetCore)seçin.</span><span class="sxs-lookup"><span data-stu-id="68cb1-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="68cb1-115">Örneğin, `Tag:v3.0.0`</span><span class="sxs-lookup"><span data-stu-id="68cb1-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="68cb1-116">[ASP.NET Core deposunu](https://github.com/aspnet/AspNetCore)kopyalayın veya indirin.</span><span class="sxs-lookup"><span data-stu-id="68cb1-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="68cb1-117">ASP.NET Core deposunun kopyası ile eşleşen [.NET Core SDK](https://www.microsoft.com/net/download/all) sürümünü yüklediğinizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="68cb1-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="68cb1-118">*Aspnetcore/src/Security/Samples* içindeki bir örneğe gidin ve örneği ile `dotnet run`çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="68cb1-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="68cb1-119">[ASP.NET Core deposu](https://github.com/aspnet/AspNetCore) , *aspnetcore/src/Security/Samples* klasöründe aşağıdaki kimlik doğrulama örneklerini içerir:</span><span class="sxs-lookup"><span data-stu-id="68cb1-119">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="68cb1-120">Talep dönüştürme</span><span class="sxs-lookup"><span data-stu-id="68cb1-120">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="68cb1-121">Tanımlama bilgisi kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="68cb1-121">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="68cb1-122">Özel ilke sağlayıcısı-ıauthorizationpolicyprovider</span><span class="sxs-lookup"><span data-stu-id="68cb1-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="68cb1-123">Dinamik kimlik doğrulama şemaları ve seçenekleri</span><span class="sxs-lookup"><span data-stu-id="68cb1-123">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="68cb1-124">Dış talepler</span><span class="sxs-lookup"><span data-stu-id="68cb1-124">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="68cb1-125">İsteğe bağlı tanımlama bilgisi ve başka bir kimlik doğrulama düzeni arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="68cb1-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="68cb1-126">Statik dosyalara erişimi kısıtlar</span><span class="sxs-lookup"><span data-stu-id="68cb1-126">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="68cb1-127">Örnekleri çalıştırın</span><span class="sxs-lookup"><span data-stu-id="68cb1-127">Run the samples</span></span>

* <span data-ttu-id="68cb1-128">Bir [dal](https://github.com/aspnet/AspNetCore)seçin.</span><span class="sxs-lookup"><span data-stu-id="68cb1-128">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="68cb1-129">Örneğin, `release/2.2`</span><span class="sxs-lookup"><span data-stu-id="68cb1-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="68cb1-130">[ASP.NET Core deposunu](https://github.com/aspnet/AspNetCore)kopyalayın veya indirin.</span><span class="sxs-lookup"><span data-stu-id="68cb1-130">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="68cb1-131">ASP.NET Core deposunun kopyası ile eşleşen [.NET Core SDK](https://www.microsoft.com/net/download/all) sürümünü yüklediğinizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="68cb1-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="68cb1-132">*Aspnetcore/src/Security/Samples* içindeki bir örneğe gidin ve örneği ile `dotnet run`çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="68cb1-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
