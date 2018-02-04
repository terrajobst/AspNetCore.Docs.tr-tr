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
ms.openlocfilehash: 3b72cddb7a240ad6d6e1427796e9bb4f7003a3f7
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Bir ASP.NET Core uygulamada SSL zorlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belge gösterir nasıl yapılır:

- SSL için tüm istekleri (yalnızca HTTPS istekleri) gerektirir.
- Tüm HTTP isteklerini yeniden yönlendirmek için HTTPS.

## <a name="require-ssl"></a>SSL iste

[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) SSL gerektirmek için kullanılır. Denetleyicileri veya bu öznitelik yöntemleriyle işaretleme veya aşağıda gösterildiği gibi genel uygulayabilirsiniz:

Aşağıdaki kodu ekleyin `ConfigureServices` içinde `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Tüm istekleri kullanır yukarıdaki vurgulanmış kodu gerektirir `HTTPS`, bu nedenle HTTP isteklerini göz ardı edilir. Aşağıdaki vurgulanmış kodu tüm HTTP istekleri için HTTPS yönlendirir:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting) daha fazla bilgi için.

HTTPS genel gerektiren (`options.Filters.Add(new RequireHttpsAttribute());`) bir güvenlik en iyi uygulamadır. Uygulama `[RequireHttps]` tüm denetleyicisine özniteliği değil olarak kabul güvenli olarak genel HTTPS gerektiren. Garanti edemez, uygulamanızın eklenen yeni denetleyicileri unutmayın uygulamak `[RequireHttps]` özniteliği.
