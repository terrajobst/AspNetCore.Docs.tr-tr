---
title: ASP.NET Core ilke düzenleri
author: rick-anderson
description: Kimlik doğrulama İlkesi düzenleri tek bir mantıksal kimlik doğrulaması düzenine sahip kolaylaştırır
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: c310b61e14df2b7846e32a602bb75914a5850aff
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901649"
---
# <a name="policy-schemes-in-aspnet-core"></a>ASP.NET Core ilke düzenleri

Kimlik doğrulama İlkesi düzenleri potansiyel olarak birden çok yaklaşımın kullanan bir mantıksal tek bir kimlik doğrulaması düzeni kolaylaştırır. Örneğin, bir ilke düzeni zorlukları için Google kimlik doğrulama ve tanımlama bilgisi kimlik doğrulamasını her şey için kullanabilirsiniz. Kimlik doğrulama İlkesi düzenleri kolaylaştırır:

* Başka bir şema için herhangi bir kimlik doğrulama eylemi iletmek kolaylaştırır.
* Dinamik olarak ve isteğe bağlı İleri.

Kullanım türetilmiş tüm kimlik doğrulama düzenleri <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> ve ilişkili [ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):

* Otomatik olarak ilke düzenleri ASP.NET Core 2.1 ve üzeri olan.
* Yapılandırma Şeması'nın seçenekleri aracılığıyla etkinleştirilebilir.

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, alt düzey şemalarını birleştiren daha yüksek düzeyli bir şeması gösterir. Google kimlik doğrulama sorunları için kullanılır ve tanımlama bilgisi kimlik doğrulamasını her şey için kullanılır:

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

Aşağıdaki örnek, istek başına temelinde düzenleri dinamik seçimini etkinleştirir. Diğer bir deyişle, tanımlama bilgileri ve API kimlik doğrulamasını karıştırma nasıl.

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
