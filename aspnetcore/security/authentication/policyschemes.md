---
title: ASP.NET Core 'de ilke şemaları
author: rick-anderson
description: Kimlik doğrulama ilkesi şemaları, tek bir mantıksal kimlik doğrulama şemasına sahip olmasını kolaylaştırır
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: f02d8e5cac20a9b60c5eddbd28253efacf682ea1
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880712"
---
# <a name="policy-schemes-in-aspnet-core"></a><span data-ttu-id="bd443-103">ASP.NET Core 'de ilke şemaları</span><span class="sxs-lookup"><span data-stu-id="bd443-103">Policy schemes in ASP.NET Core</span></span>

<span data-ttu-id="bd443-104">Kimlik doğrulama ilkesi şemaları, tek bir mantıksal kimlik doğrulama düzeninin birden çok yaklaşım kullanmasına daha kolay olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="bd443-104">Authentication policy schemes make it easier to have a single logical authentication scheme potentially use multiple approaches.</span></span> <span data-ttu-id="bd443-105">Örneğin, bir ilke şeması, sorunlar için Google kimlik doğrulaması ve diğer her şey için tanımlama bilgisi kimlik doğrulaması kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="bd443-105">For example, a policy scheme might use Google authentication for challenges, and cookie authentication for everything else.</span></span> <span data-ttu-id="bd443-106">Kimlik doğrulama ilkesi şemaları şunları yapar:</span><span class="sxs-lookup"><span data-stu-id="bd443-106">Authentication policy schemes make it:</span></span>

* <span data-ttu-id="bd443-107">Herhangi bir kimlik doğrulama eylemini başka bir düzene iletmek kolay.</span><span class="sxs-lookup"><span data-stu-id="bd443-107">Easy to forward any authentication action to another scheme.</span></span>
* <span data-ttu-id="bd443-108">İsteğe göre dinamik olarak ilet.</span><span class="sxs-lookup"><span data-stu-id="bd443-108">Forward dynamically based on the request.</span></span>

<span data-ttu-id="bd443-109">Türetilmiş <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> ve ilişkili [Authenticationhandler\<TOptions >](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1)kullanan tüm kimlik doğrulama şemaları:</span><span class="sxs-lookup"><span data-stu-id="bd443-109">All authentication schemes that use derived <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> and the associated [AuthenticationHandler\<TOptions>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span></span>

* <span data-ttu-id="bd443-110">ASP.NET Core 2,1 ve sonraki sürümlerde otomatik olarak ilke düzenleridir.</span><span class="sxs-lookup"><span data-stu-id="bd443-110">Are automatically policy schemes in ASP.NET Core 2.1 and later.</span></span>
* <span data-ttu-id="bd443-111">Düzenin seçenekleri yapılandırılarak etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="bd443-111">Can be enabled via configuring the scheme's options.</span></span>

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a><span data-ttu-id="bd443-112">Örnekler</span><span class="sxs-lookup"><span data-stu-id="bd443-112">Examples</span></span>

<span data-ttu-id="bd443-113">Aşağıdaki örnek, alt düzey şemaları birleştiren daha yüksek düzey bir düzeni gösterir.</span><span class="sxs-lookup"><span data-stu-id="bd443-113">The following example shows a higher level scheme that combines lower level schemes.</span></span> <span data-ttu-id="bd443-114">Google kimlik doğrulaması, sorunlar için kullanılır ve diğer her şey için tanımlama bilgisi kimlik doğrulaması kullanılır:</span><span class="sxs-lookup"><span data-stu-id="bd443-114">Google authentication is used for challenges, and cookie authentication is used for everything else:</span></span>

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="bd443-115">Aşağıdaki örnek, istek temelinde dinamik düzen seçimini mümkün bir şekilde sunar.</span><span class="sxs-lookup"><span data-stu-id="bd443-115">The following example enables dynamic selection of schemes on a per request basis.</span></span> <span data-ttu-id="bd443-116">Diğer bir deyişle, tanımlama bilgilerini ve API kimlik doğrulamasını karıştırma:</span><span class="sxs-lookup"><span data-stu-id="bd443-116">That is, how to mix cookies and API authentication:</span></span>

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
