---
title: ASP.NET Core kısa ömürlü veri koruma sağlayıcıları
author: rick-anderson
description: ASP.NET Core kısa ömürlü veri koruma sağlayıcılarının uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: e4b0014ab3bdbf90b91383e8a33102f94faa8153
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664740"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>ASP.NET Core kısa ömürlü veri koruma sağlayıcıları

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Bir uygulamanın bir throwaway `IDataProtectionProvider`ihtiyaç duyacağı senaryolar vardır. Örneğin, geliştirici yalnızca tek bir konsol uygulamasında çalışabilir ya da uygulamanın kendisi geçici (betikleştirilmiş veya bir birim test projesi). Bu senaryoları desteklemek için [Microsoft. AspNetCore. DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) paketi `EphemeralDataProtectionProvider`bir tür içerir. Bu tür, anahtar deposunun yalnızca bellek içinde tutulduğu ve herhangi bir yedekleme deposuna yazılmadığı `IDataProtectionProvider` temel bir uygulamasını sağlar.

Her `EphemeralDataProtectionProvider` örneği kendi benzersiz ana anahtarını kullanır. Bu nedenle, bir `EphemeralDataProtectionProvider` kökü olan bir `IDataProtector` korunan bir yük oluşturursa, bu yük yalnızca aynı `EphemeralDataProtectionProvider` örneğinde belirtilen eşdeğer bir `IDataProtector` (aynı [Amaç](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) zinciri olarak) tarafından korumasız olabilir.

Aşağıdaki örnek, `EphemeralDataProtectionProvider` örneğini oluşturma ve verileri korumak ve korumayı kaldırmak için kullanmayı göstermektedir.

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
