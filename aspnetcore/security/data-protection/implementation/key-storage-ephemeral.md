---
title: ASP.NET Core kısa ömürlü veri koruma sağlayıcıları
author: rick-anderson
description: Uygulama Ayrıntıları ASP.NET Core kısa ömürlü veri koruma sağlayıcısı öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279474"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>ASP.NET Core kısa ömürlü veri koruma sağlayıcıları

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Burada bir uygulama gerekiyor bir nasıl senaryo vardır `IDataProtectionProvider`. Örneğin, geliştirici hemen bir kerelik konsol uygulamasında denemeler veya uygulama geçici (Bu komut dosyası veya bir birim testi projesi). Bu senaryoları desteklemek için [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) paketi içeren bir türü `EphemeralDataProtectionProvider`. Bu tür bir temel uygulamasını sağlar `IDataProtectionProvider` , anahtar deposu yalnızca bellek tutulur ve tüm yedekleme deposu için yazılan değil.

Her bir örneğini `EphemeralDataProtectionProvider` kendi benzersiz ana anahtarı kullanır. Bu nedenle, varsa bir `IDataProtector` köklü bir `EphemeralDataProtectionProvider` korumalı bir yük oluşturur, yükü yalnızca bir eşdeğerleriyle korumasız `IDataProtector` (aynı verilen [amacı](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) zinciri) aynı anda kökü `EphemeralDataProtectionProvider` örneği.

Aşağıdaki örnek başlatmasını gösterir bir `EphemeralDataProtectionProvider` ve korumak ve veri korumasını kaldırmak için kullanma.

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
