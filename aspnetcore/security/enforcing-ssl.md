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
ms.openlocfilehash: f248e9c0463cf4a46a447a9c896b3276a50f5f08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="aea77-103">Bir ASP.NET Core uygulamada SSL zorlama</span><span class="sxs-lookup"><span data-stu-id="aea77-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="aea77-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aea77-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aea77-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="aea77-105">This document shows how to:</span></span>

- <span data-ttu-id="aea77-106">SSL için tüm istekleri (yalnızca HTTPS istekleri) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="aea77-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="aea77-107">Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.</span><span class="sxs-lookup"><span data-stu-id="aea77-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="aea77-108">SSL iste</span><span class="sxs-lookup"><span data-stu-id="aea77-108">Require SSL</span></span>

<span data-ttu-id="aea77-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) SSL gerektirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aea77-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="aea77-110">Denetleyicileri veya bu öznitelik yöntemleriyle işaretleme veya aşağıda gösterildiği gibi genel uygulayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="aea77-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="aea77-111">Aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="aea77-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="aea77-112">Tüm istekleri kullanır yukarıdaki vurgulanmış kodu gerektirir `HTTPS`, bu nedenle HTTP isteklerini göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="aea77-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="aea77-113">Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="aea77-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="aea77-114">Bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="aea77-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="aea77-115">HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="aea77-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="aea77-116">Uygulama `[RequireHttps]` tüm denetleyicisine özniteliği değil olarak kabul güvenli olarak genel HTTPS gerektiren.</span><span class="sxs-lookup"><span data-stu-id="aea77-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="aea77-117">Garanti edemez, uygulamanızın eklenen yeni denetleyicileri unutmayın uygulamak `[RequireHttps]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="aea77-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
