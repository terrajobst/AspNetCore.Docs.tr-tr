---
title: ASP.NET core'da kısa ömürlü veri koruma sağlayıcıları
author: rick-anderson
description: ASP.NET Core kısa ömürlü veri koruma sağlayıcıları uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901628"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>ASP.NET core'da kısa ömürlü veri koruma sağlayıcıları

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Bir uygulama bir nasıl gereken yere senaryo vardır `IDataProtectionProvider`. Örneğin, geliştirici yalnızca bir tek seferlik bir konsol uygulamasında denemeler veya uygulama geçici (Bu komut dosyası veya bir birim test projesi). Bu senaryoları desteklemek için [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) paketi içeren bir tür `EphemeralDataProtectionProvider`. Bu tür temel bir uygulamasını sağlar `IDataProtectionProvider` , anahtar deposu yalnızca bellek içi tutulur ve herhangi bir yedekleme deposu için yazılan değil.

Her bir örneği `EphemeralDataProtectionProvider` kendi benzersiz ana anahtarı kullanır. Bu nedenle, bir `IDataProtector` köklü bir `EphemeralDataProtectionProvider` korumalı bir yük oluşturur bu yükü yalnızca bir eşdeğeri tarafından korunmayan `IDataProtector` (aynı verilen [amaçlı](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) zinciri) aynı anda kökü `EphemeralDataProtectionProvider` örneği.

Aşağıdaki örnek, örnekleme gösterir. bir `EphemeralDataProtectionProvider` ve korumaya ve veri korumasını kaldırmak için kullanma.

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
