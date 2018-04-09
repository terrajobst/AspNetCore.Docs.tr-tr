---
title: Bir ASP.NET Core HTTPS zorla
author: rick-anderson
description: Bir ASP.NET Core HTTPS/TLS gerektiren web uygulaması gösterilmektedir.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2ebb975e1ea17698cee13ca00d3f5df4a5135e38
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="3080e-103">Bir ASP.NET Core HTTPS zorla</span><span class="sxs-lookup"><span data-stu-id="3080e-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="3080e-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3080e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3080e-105">Bu belge gösterir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="3080e-105">This document shows how to:</span></span>

- <span data-ttu-id="3080e-106">HTTPS için tüm istekleri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3080e-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="3080e-107">Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3080e-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="3080e-108">Yapmak **değil** kullanmak `RequireHttpsAttribute` Web API'lerde hassas bilgiler alırsınız.</span><span class="sxs-lookup"><span data-stu-id="3080e-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="3080e-109">`RequireHttpsAttribute` HTTP tarayıcılarından HTTPS'ye yeniden yönlendirmek için HTTP durum kodları kullanır.</span><span class="sxs-lookup"><span data-stu-id="3080e-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="3080e-110">API istemcileri değil anlamak veya HTTP yönlendirir HTTPS uymaktadır.</span><span class="sxs-lookup"><span data-stu-id="3080e-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="3080e-111">Bu tür istemciler HTTP üzerinden bilgi gönderebilir.</span><span class="sxs-lookup"><span data-stu-id="3080e-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="3080e-112">Web API'leri aşağıdakilerden birini yapmalısınız:</span><span class="sxs-lookup"><span data-stu-id="3080e-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="3080e-113">HTTP dinleme değil.</span><span class="sxs-lookup"><span data-stu-id="3080e-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="3080e-114">Durum kodu 400 (Hatalı istek) ile bağlantı kapatın ve istek hizmet yok.</span><span class="sxs-lookup"><span data-stu-id="3080e-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="3080e-115">HTTPS gerektirir</span><span class="sxs-lookup"><span data-stu-id="3080e-115">Require HTTPS</span></span>

<span data-ttu-id="3080e-116">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) HTTPS gerektirecek şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3080e-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="3080e-117">`[RequireHttpsAttribute]` denetleyicileri veya yöntemleri işaretleme veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="3080e-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="3080e-118">Öznitelik genel uygulamak için aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:</span><span class="sxs-lookup"><span data-stu-id="3080e-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="3080e-119">Tüm istekleri kullanır önceki vurgulanmış kodu gerektirir `HTTPS`; bu nedenle, HTTP isteklerini yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="3080e-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="3080e-120">Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:</span><span class="sxs-lookup"><span data-stu-id="3080e-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="3080e-121">Daha fazla bilgi için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="3080e-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="3080e-122">HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="3080e-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="3080e-123">Uygulama `[RequireHttps]` tüm denetleyicileri/Razor sayfalarının özniteliğine değil olarak kabul güvenli olarak genel HTTPS gerektiren.</span><span class="sxs-lookup"><span data-stu-id="3080e-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="3080e-124">Garanti edemez `[RequireHttps]` özniteliği yeni denetleyicileri ve Razor sayfalarının eklendiğinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="3080e-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>
