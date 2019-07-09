---
title: ASP.NET Core ilke düzenleri
author: rick-anderson
description: Kimlik doğrulama İlkesi düzenleri tek bir mantıksal kimlik doğrulaması düzenine sahip kolaylaştırır
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: 1a2d92e6fa54189b8154fc501b31c8a99d1f9081
ms.sourcegitcommit: 357a7120632b20465801c093e4e5bd4a315496a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649175"
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

Aşağıdaki örnek, istek başına temelinde düzenleri dinamik seçimini etkinleştirir. Diğer bir deyişle, tanımlama bilgileri ve API kimlik doğrulamasını karıştırma nasıl:

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
