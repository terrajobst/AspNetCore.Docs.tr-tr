---
title: Bir ASP.NET Core uygulamada SSL zorlama
author: rick-anderson
description: "Web uygulaması ASP.NET Core SSL gerektirecek şekilde nasıl gösterir"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 42d8767fda2d3f3545876f8ca18e0f6fbe6741b8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="7026c-103">Bir ASP.NET Core uygulamada SSL zorlama</span><span class="sxs-lookup"><span data-stu-id="7026c-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="7026c-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7026c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7026c-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="7026c-105">This document shows how to:</span></span>

- <span data-ttu-id="7026c-106">SSL için tüm istekleri (yalnızca HTTPS istekleri) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7026c-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="7026c-107">Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7026c-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="7026c-108">SSL iste</span><span class="sxs-lookup"><span data-stu-id="7026c-108">Require SSL</span></span>

<span data-ttu-id="7026c-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) SSL gerektirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7026c-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="7026c-110">Denetleyicileri veya bu öznitelik yöntemleriyle işaretleme veya aşağıda gösterildiği gibi genel uygulayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7026c-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="7026c-111">Aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7026c-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="7026c-112">Tüm istekleri kullanır yukarıdaki vurgulanmış kodu gerektirir `HTTPS`, bu nedenle HTTP isteklerini göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="7026c-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="7026c-113">Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="7026c-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="7026c-114">Bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="7026c-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="7026c-115">HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="7026c-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="7026c-116">Uygulama `[RequireHttps]` tüm denetleyicisine özniteliği değil genel HTTPS gerektiren güvenli olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="7026c-116">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="7026c-117">Garanti edemez, uygulamanızın eklenen yeni denetleyicileri unutmayın uygulamak `[RequireHttps]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="7026c-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
