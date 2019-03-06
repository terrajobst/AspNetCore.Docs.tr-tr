---
title: ASP.NET Core ilke düzenleri
author: rick-anderson
description: Kimlik doğrulama İlkesi düzenleri tek bir mantıksal kimlik doğrulaması düzenine sahip kolaylaştırır
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: c310b61e14df2b7846e32a602bb75914a5850aff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346816"
---
# <a name="policy-schemes-in-aspnet-core"></a><span data-ttu-id="4fefc-103">ASP.NET Core ilke düzenleri</span><span class="sxs-lookup"><span data-stu-id="4fefc-103">Policy schemes in ASP.NET Core</span></span>

<span data-ttu-id="4fefc-104">Kimlik doğrulama İlkesi düzenleri potansiyel olarak birden çok yaklaşımın kullanan bir mantıksal tek bir kimlik doğrulaması düzeni kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="4fefc-104">Authentication policy schemes make it easier to have a single logical authentication scheme potentially use multiple approaches.</span></span> <span data-ttu-id="4fefc-105">Örneğin, bir ilke düzeni zorlukları için Google kimlik doğrulama ve tanımlama bilgisi kimlik doğrulamasını her şey için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4fefc-105">For example, a policy scheme might use Google authentication for challenges, and cookie authentication for everything else.</span></span> <span data-ttu-id="4fefc-106">Kimlik doğrulama İlkesi düzenleri kolaylaştırır:</span><span class="sxs-lookup"><span data-stu-id="4fefc-106">Authentication policy schemes make it:</span></span>

* <span data-ttu-id="4fefc-107">Başka bir şema için herhangi bir kimlik doğrulama eylemi iletmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="4fefc-107">Easy to forward any authentication action to another scheme.</span></span>
* <span data-ttu-id="4fefc-108">Dinamik olarak ve isteğe bağlı İleri.</span><span class="sxs-lookup"><span data-stu-id="4fefc-108">Forward dynamically based on the request.</span></span>

<span data-ttu-id="4fefc-109">Kullanım türetilmiş tüm kimlik doğrulama düzenleri <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> ve ilişkili [ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span><span class="sxs-lookup"><span data-stu-id="4fefc-109">All authentication schemes that use derived <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> and the associated [`AuthenticationHandler<TOptions>`](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span></span>

* <span data-ttu-id="4fefc-110">Otomatik olarak ilke düzenleri ASP.NET Core 2.1 ve üzeri olan.</span><span class="sxs-lookup"><span data-stu-id="4fefc-110">Are automatically policy schemes in ASP.NET Core 2.1 and later.</span></span>
* <span data-ttu-id="4fefc-111">Yapılandırma Şeması'nın seçenekleri aracılığıyla etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="4fefc-111">Can be enabled via configuring the scheme's options.</span></span>

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a><span data-ttu-id="4fefc-112">Örnekler</span><span class="sxs-lookup"><span data-stu-id="4fefc-112">Examples</span></span>

<span data-ttu-id="4fefc-113">Aşağıdaki örnek, alt düzey şemalarını birleştiren daha yüksek düzeyli bir şeması gösterir.</span><span class="sxs-lookup"><span data-stu-id="4fefc-113">The following example shows a higher level scheme that combines lower level schemes.</span></span> <span data-ttu-id="4fefc-114">Google kimlik doğrulama sorunları için kullanılır ve tanımlama bilgisi kimlik doğrulamasını her şey için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="4fefc-114">Google authentication is used for challenges, and cookie authentication is used for everything else:</span></span>

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="4fefc-115">Aşağıdaki örnek, istek başına temelinde düzenleri dinamik seçimini etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="4fefc-115">The following example enables dynamic selection of schemes on a per request basis.</span></span> <span data-ttu-id="4fefc-116">Diğer bir deyişle, tanımlama bilgileri ve API kimlik doğrulamasını karıştırma nasıl.</span><span class="sxs-lookup"><span data-stu-id="4fefc-116">That is, how to mix cookies and API authentication.</span></span>

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
