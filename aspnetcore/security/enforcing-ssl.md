---
title: Bir ASP.NET Core uygulamada SSL zorlama
author: rick-anderson
description: "Web uygulaması ASP.NET Core SSL gerektirecek şekilde nasıl gösterir"
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2e0a2f4732e574c80ceef8fd21a530a11aef254c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="63007-103">Bir ASP.NET Core uygulamada SSL zorlama</span><span class="sxs-lookup"><span data-stu-id="63007-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="63007-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="63007-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="63007-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="63007-105">This document shows how to:</span></span>

- <span data-ttu-id="63007-106">SSL için tüm istekleri (yalnızca HTTPS istekleri) gerektirir.</span><span class="sxs-lookup"><span data-stu-id="63007-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="63007-107">Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.</span><span class="sxs-lookup"><span data-stu-id="63007-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="63007-108">SSL iste</span><span class="sxs-lookup"><span data-stu-id="63007-108">Require SSL</span></span>

<span data-ttu-id="63007-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) SSL gerektirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="63007-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="63007-110">Denetleyicileri veya bu öznitelik yöntemleriyle işaretleme veya aşağıda gösterildiği gibi genel uygulayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="63007-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="63007-111">Aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="63007-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="63007-112">Tüm istekleri kullanır yukarıdaki vurgulanmış kodu gerektirir `HTTPS`, bu nedenle HTTP isteklerini göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="63007-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="63007-113">Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="63007-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="63007-114">Bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="63007-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="63007-115">HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="63007-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="63007-116">Uygulama `[RequireHttps]` tüm denetleyicisine özniteliği değil olarak kabul güvenli olarak genel HTTPS gerektiren.</span><span class="sxs-lookup"><span data-stu-id="63007-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="63007-117">Garanti edemez, uygulamanızın eklenen yeni denetleyicileri unutmayın uygulamak `[RequireHttps]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="63007-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
