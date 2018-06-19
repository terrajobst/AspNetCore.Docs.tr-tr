---
title: ASP.NET Core kısa ömürlü veri koruma sağlayıcıları
author: rick-anderson
description: Uygulama Ayrıntıları ASP.NET Core kısa ömürlü veri koruma sağlayıcısı öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9cdd3949826091807f3139f51aa0c05ddaac696a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30076140"
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a><span data-ttu-id="a1031-103">ASP.NET Core kısa ömürlü veri koruma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="a1031-103">Ephemeral data protection providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="a1031-104">Burada bir uygulama gerekiyor bir nasıl senaryo vardır `IDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="a1031-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="a1031-105">Örneğin, geliştirici hemen bir kerelik konsol uygulamasında denemeler veya uygulama geçici (Bu komut dosyası veya bir birim testi projesi).</span><span class="sxs-lookup"><span data-stu-id="a1031-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="a1031-106">Bu senaryoları desteklemek için [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) paketi içeren bir türü `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="a1031-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="a1031-107">Bu tür bir temel uygulamasını sağlar `IDataProtectionProvider` , anahtar deposu yalnızca bellek tutulur ve tüm yedekleme deposu için yazılan değil.</span><span class="sxs-lookup"><span data-stu-id="a1031-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="a1031-108">Her bir örneğini `EphemeralDataProtectionProvider` kendi benzersiz ana anahtarı kullanır.</span><span class="sxs-lookup"><span data-stu-id="a1031-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="a1031-109">Bu nedenle, varsa bir `IDataProtector` köklü bir `EphemeralDataProtectionProvider` korumalı bir yük oluşturur, yükü yalnızca bir eşdeğerleriyle korumasız `IDataProtector` (aynı verilen [amacı](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) zinciri) aynı anda kökü `EphemeralDataProtectionProvider` örneği.</span><span class="sxs-lookup"><span data-stu-id="a1031-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="a1031-110">Aşağıdaki örnek başlatmasını gösterir bir `EphemeralDataProtectionProvider` ve korumak ve veri korumasını kaldırmak için kullanma.</span><span class="sxs-lookup"><span data-stu-id="a1031-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

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
