---
title: ASP.NET Core 'de ilke şemaları
author: rick-anderson
description: Kimlik doğrulama ilkesi şemaları, tek bir mantıksal kimlik doğrulama şemasına sahip olmasını kolaylaştırır
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: f02d8e5cac20a9b60c5eddbd28253efacf682ea1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660736"
---
# <a name="policy-schemes-in-aspnet-core"></a>ASP.NET Core 'de ilke şemaları

Kimlik doğrulama ilkesi şemaları, tek bir mantıksal kimlik doğrulama düzeninin birden çok yaklaşım kullanmasına daha kolay olmasını sağlar. Örneğin, bir ilke şeması, sorunlar için Google kimlik doğrulaması ve diğer her şey için tanımlama bilgisi kimlik doğrulaması kullanabilir. Kimlik doğrulama ilkesi şemaları şunları yapar:

* Herhangi bir kimlik doğrulama eylemini başka bir düzene iletmek kolay.
* İsteğe göre dinamik olarak ilet.

Türetilmiş <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> ve ilişkili [Authenticationhandler\<TOptions >](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1)kullanan tüm kimlik doğrulama şemaları:

* ASP.NET Core 2,1 ve sonraki sürümlerde otomatik olarak ilke düzenleridir.
* Düzenin seçenekleri yapılandırılarak etkinleştirilebilir.

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, alt düzey şemaları birleştiren daha yüksek düzey bir düzeni gösterir. Google kimlik doğrulaması, sorunlar için kullanılır ve diğer her şey için tanımlama bilgisi kimlik doğrulaması kullanılır:

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

Aşağıdaki örnek, istek temelinde dinamik düzen seçimini mümkün bir şekilde sunar. Diğer bir deyişle, tanımlama bilgilerini ve API kimlik doğrulamasını karıştırma:

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
