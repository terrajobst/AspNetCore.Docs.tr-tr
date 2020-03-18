---
title: ASP.NET Core için kimlik doğrulama örnekleri
author: rick-anderson
description: ASP.NET Core deposundaki kimlik doğrulama örneklerine bağlantılar sağlar.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: b013d02a65e752bbb61a87a0bf502785125378a2
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511619"
---
# <a name="authentication-samples-for-aspnet-core"></a>ASP.NET Core için kimlik doğrulama örnekleri

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[ASP.NET Core deposu](https://github.com/dotnet/AspNetCore) , *aspnetcore/src/Security/Samples* klasöründe aşağıdaki kimlik doğrulama örneklerini içerir:

* [Talep dönüştürme](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [Tanımlama bilgisi kimlik doğrulaması](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [Özel ilke sağlayıcısı-ıauthorizationpolicyprovider](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [Dinamik kimlik doğrulama şemaları ve seçenekleri](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [Dış talepler](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [İsteğe bağlı tanımlama bilgisi ve başka bir kimlik doğrulama düzeni arasında seçim yapma](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [Statik dosyalara erişimi kısıtlar](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Örnekleri çalıştırma

* Bir [dal](https://github.com/dotnet/AspNetCore)seçin. Örneğin, `Tag:v3.0.0`
* [ASP.NET Core deposunu](https://github.com/dotnet/AspNetCore)kopyalayın veya indirin.
* ASP.NET Core deposunun kopyası ile eşleşen [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) sürümünü yüklediğinizi doğrulayın.
* *Aspnetcore/src/Security/Samples* içindeki bir örneğe gidin ve `dotnet run`örneği çalıştırın.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[ASP.NET Core deposu](https://github.com/dotnet/AspNetCore) , *aspnetcore/src/Security/Samples* klasöründe aşağıdaki kimlik doğrulama örneklerini içerir:

* [Talep dönüştürme](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Tanımlama bilgisi kimlik doğrulaması](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Özel ilke sağlayıcısı-ıauthorizationpolicyprovider](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Dinamik kimlik doğrulama şemaları ve seçenekleri](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Dış talepler](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [İsteğe bağlı tanımlama bilgisi ve başka bir kimlik doğrulama düzeni arasında seçim yapma](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Statik dosyalara erişimi kısıtlar](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Örnekleri çalıştırma

* Bir [dal](https://github.com/dotnet/AspNetCore)seçin. Örneğin, `release/2.2`
* [ASP.NET Core deposunu](https://github.com/dotnet/AspNetCore)kopyalayın veya indirin.
* ASP.NET Core deposunun kopyası ile eşleşen [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) sürümünü yüklediğinizi doğrulayın.
* *Aspnetcore/src/Security/Samples* içindeki bir örneğe gidin ve `dotnet run`örneği çalıştırın.

::: moniker-end
