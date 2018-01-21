---
title: "Kısa ömürlü veri koruma sağlayıcısı"
author: rick-anderson
description: "Bu belgede ASP.NET Core kısa ömürlü veri koruma sağlayıcısı uygulama ayrıntıları açıklanmaktadır."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9c1d03373c9d7fb6dffb3583c58aa593fd3875f4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="ephemeral-data-protection-providers"></a><span data-ttu-id="86d3a-103">Kısa ömürlü veri koruma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="86d3a-103">Ephemeral data protection providers</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="86d3a-104">Burada bir uygulama gerekiyor bir nasıl senaryo vardır `IDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="86d3a-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="86d3a-105">Örneğin, geliştirici hemen bir kerelik konsol uygulamasında denemeler veya uygulama geçici (Bu komut dosyası veya bir birim testi projesi).</span><span class="sxs-lookup"><span data-stu-id="86d3a-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="86d3a-106">Bu senaryoları desteklemek için [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) paketi içeren bir türü `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="86d3a-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="86d3a-107">Bu tür bir temel uygulamasını sağlar `IDataProtectionProvider` , anahtar deposu yalnızca bellek tutulur ve tüm yedekleme deposu için yazılan değil.</span><span class="sxs-lookup"><span data-stu-id="86d3a-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="86d3a-108">Her bir örneğini `EphemeralDataProtectionProvider` kendi benzersiz ana anahtarı kullanır.</span><span class="sxs-lookup"><span data-stu-id="86d3a-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="86d3a-109">Bu nedenle, varsa bir `IDataProtector` köklü bir `EphemeralDataProtectionProvider` korumalı bir yük oluşturur, yükü yalnızca bir eşdeğerleriyle korumasız `IDataProtector` (aynı verilen [amacı](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) zinciri) aynı anda kökü `EphemeralDataProtectionProvider` örneği.</span><span class="sxs-lookup"><span data-stu-id="86d3a-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="86d3a-110">Aşağıdaki örnek başlatmasını gösterir bir `EphemeralDataProtectionProvider` ve korumak ve veri korumasını kaldırmak için kullanma.</span><span class="sxs-lookup"><span data-stu-id="86d3a-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

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
