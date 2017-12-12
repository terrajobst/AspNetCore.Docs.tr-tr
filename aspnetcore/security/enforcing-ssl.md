---
title: Bir ASP.NET Core uygulamada SSL zorlama
author: rick-anderson
description: "Web uygulaması ASP.NET Core SSL gerektirecek şekilde nasıl gösterir"
keywords: ASP.NET Core, SSL, HTTPS, RequireHttpsAttribute, IIS Express
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Bir ASP.NET Core uygulamada SSL zorlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belge gösterir nasıl yapılır:

- SSL için tüm istekleri (yalnızca HTTPS istekleri) gerektirir.
- Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.

## <a name="require-ssl"></a>SSL iste

[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) SSL gerektirmek için kullanılır. Denetleyicileri veya bu öznitelik yöntemleriyle işaretleme veya aşağıda gösterildiği gibi genel uygulayabilirsiniz:

Aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

Tüm istekleri kullanır yukarıdaki vurgulanmış kodu gerektirir `HTTPS`, bu nedenle HTTP isteklerini göz ardı edilir. Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

Bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting) daha fazla bilgi için.

HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır. Uygulama `[RequireHttps]` tüm denetleyicisine özniteliği değil genel HTTPS gerektiren güvenli olarak değerlendirilir. Garanti edemez, uygulamanızın eklenen yeni denetleyicileri unutmayın uygulamak `[RequireHttps]` özniteliği.
