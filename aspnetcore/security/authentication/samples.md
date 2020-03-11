---
title: ASP.NET Core için kimlik doğrulama örnekleri
author: rick-anderson
description: ASP.NET Core deposundaki kimlik doğrulama örneklerine bağlantılar sağlar.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 3d7e28f6e501bd8bd3908ca4b314a63cee52ebe3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658419"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="99b5c-103">ASP.NET Core için kimlik doğrulama örnekleri</span><span class="sxs-lookup"><span data-stu-id="99b5c-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="99b5c-104">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="99b5c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="99b5c-105">[ASP.NET Core deposu](https://github.com/dotnet/AspNetCore) , *aspnetcore/src/Security/Samples* klasöründe aşağıdaki kimlik doğrulama örneklerini içerir:</span><span class="sxs-lookup"><span data-stu-id="99b5c-105">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="99b5c-106">Talep dönüştürme</span><span class="sxs-lookup"><span data-stu-id="99b5c-106">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="99b5c-107">Tanımlama bilgisi kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="99b5c-107">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="99b5c-108">Özel ilke sağlayıcısı-ıauthorizationpolicyprovider</span><span class="sxs-lookup"><span data-stu-id="99b5c-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="99b5c-109">Dinamik kimlik doğrulama şemaları ve seçenekleri</span><span class="sxs-lookup"><span data-stu-id="99b5c-109">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="99b5c-110">Dış talepler</span><span class="sxs-lookup"><span data-stu-id="99b5c-110">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="99b5c-111">İsteğe bağlı tanımlama bilgisi ve başka bir kimlik doğrulama düzeni arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="99b5c-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="99b5c-112">Statik dosyalara erişimi kısıtlar</span><span class="sxs-lookup"><span data-stu-id="99b5c-112">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="99b5c-113">Örnekleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="99b5c-113">Run the samples</span></span>

* <span data-ttu-id="99b5c-114">Bir [dal](https://github.com/dotnet/AspNetCore)seçin.</span><span class="sxs-lookup"><span data-stu-id="99b5c-114">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="99b5c-115">Örneğin, `Tag:v3.0.0`</span><span class="sxs-lookup"><span data-stu-id="99b5c-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="99b5c-116">[ASP.NET Core deposunu](https://github.com/dotnet/AspNetCore)kopyalayın veya indirin.</span><span class="sxs-lookup"><span data-stu-id="99b5c-116">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="99b5c-117">ASP.NET Core deposunun kopyası ile eşleşen [.NET Core SDK](https://www.microsoft.com/net/download/all) sürümünü yüklediğinizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="99b5c-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="99b5c-118">*Aspnetcore/src/Security/Samples* içindeki bir örneğe gidin ve `dotnet run`örneği çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="99b5c-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="99b5c-119">[ASP.NET Core deposu](https://github.com/dotnet/AspNetCore) , *aspnetcore/src/Security/Samples* klasöründe aşağıdaki kimlik doğrulama örneklerini içerir:</span><span class="sxs-lookup"><span data-stu-id="99b5c-119">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="99b5c-120">Talep dönüştürme</span><span class="sxs-lookup"><span data-stu-id="99b5c-120">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="99b5c-121">Tanımlama bilgisi kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="99b5c-121">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="99b5c-122">Özel ilke sağlayıcısı-ıauthorizationpolicyprovider</span><span class="sxs-lookup"><span data-stu-id="99b5c-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="99b5c-123">Dinamik kimlik doğrulama şemaları ve seçenekleri</span><span class="sxs-lookup"><span data-stu-id="99b5c-123">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="99b5c-124">Dış talepler</span><span class="sxs-lookup"><span data-stu-id="99b5c-124">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="99b5c-125">İsteğe bağlı tanımlama bilgisi ve başka bir kimlik doğrulama düzeni arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="99b5c-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="99b5c-126">Statik dosyalara erişimi kısıtlar</span><span class="sxs-lookup"><span data-stu-id="99b5c-126">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="99b5c-127">Örnekleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="99b5c-127">Run the samples</span></span>

* <span data-ttu-id="99b5c-128">Bir [dal](https://github.com/dotnet/AspNetCore)seçin.</span><span class="sxs-lookup"><span data-stu-id="99b5c-128">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="99b5c-129">Örneğin, `release/2.2`</span><span class="sxs-lookup"><span data-stu-id="99b5c-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="99b5c-130">[ASP.NET Core deposunu](https://github.com/dotnet/AspNetCore)kopyalayın veya indirin.</span><span class="sxs-lookup"><span data-stu-id="99b5c-130">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="99b5c-131">ASP.NET Core deposunun kopyası ile eşleşen [.NET Core SDK](https://www.microsoft.com/net/download/all) sürümünü yüklediğinizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="99b5c-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="99b5c-132">*Aspnetcore/src/Security/Samples* içindeki bir örneğe gidin ve `dotnet run`örneği çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="99b5c-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
