---
title: ASP.NET Core kimlik doğrulaması örnekleri
author: rick-anderson
description: ASP.NET Core depodaki kimlik doğrulaması örneklere bağlantılar sağlar.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902936"
---
# <a name="authentication-samples-for-aspnet-core"></a>ASP.NET Core kimlik doğrulaması örnekleri

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[ASP.NET Core depo](https://github.com/aspnet/AspNetCore) aşağıdaki kimlik doğrulama örnekleri içeren *AspNetCore/src/güvenlik/samples* klasörü:

* [Talep dönüştürme](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Tanımlama bilgisi kimlik doğrulaması](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Özel ilke sağlayıcısı - IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Dinamik kimlik doğrulama düzenleri ve seçenekleri](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Dış talep](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Tanımlama bilgisi ve yanıtı temel alarak başka bir kimlik doğrulama düzeni arasında seçme](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Statik dosyalar için erişimi kısıtlar](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Örnekleri çalıştırma

* Seçin bir [dal](https://github.com/aspnet/AspNetCore). Örneğin, `release/2.2`
* Kopyala veya indir [ASP.NET Core depo](https://github.com/aspnet/AspNetCore).
* Yüklediğiniz doğrulayın [.NET Core SDK'sı](https://www.microsoft.com/net/download/all) ASP.NET Core deponun kopyasını eşleşen sürümü.
* Bir örnek gidin *AspNetCore/src/güvenlik/samples* örnekle çalıştırıp `dotnet run`.
