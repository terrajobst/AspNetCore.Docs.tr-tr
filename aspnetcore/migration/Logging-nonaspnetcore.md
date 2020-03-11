---
title: Microsoft. Extensions. Logging 2,2 2,1 veya 3,0 ' den geçiş yapın
author: pakrym
description: 2,1 ' den 2,2 ' e veya 3,0 ' de Microsoft. Extensions. Logging kullanarak bir non-ASP.NET Core uygulamasını geçirmeyi öğrenin.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659322"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="2f6b9-103">Microsoft. Extensions. Logging 2,2 2,1 veya 3,0 ' den geçiş yapın</span><span class="sxs-lookup"><span data-stu-id="2f6b9-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="2f6b9-104">Bu makalede, 2,1 ' den 2,2 ' e veya 3,0 ' den `Microsoft.Extensions.Logging` kullanan bir non-ASP.NET Core uygulamasını geçirmeye yönelik yaygın adımlar özetlenmektedir.</span><span class="sxs-lookup"><span data-stu-id="2f6b9-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="2f6b9-105">2.1’den 2.2’ye</span><span class="sxs-lookup"><span data-stu-id="2f6b9-105">2.1 to 2.2</span></span>

<span data-ttu-id="2f6b9-106">El ile `ServiceCollection` oluşturun ve `AddLogging`çağırın.</span><span class="sxs-lookup"><span data-stu-id="2f6b9-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="2f6b9-107">2,1 örnek:</span><span class="sxs-lookup"><span data-stu-id="2f6b9-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="2f6b9-108">2,2 örnek:</span><span class="sxs-lookup"><span data-stu-id="2f6b9-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="2f6b9-109">2,1 3,0</span><span class="sxs-lookup"><span data-stu-id="2f6b9-109">2.1 to 3.0</span></span>

<span data-ttu-id="2f6b9-110">3,0 ' de `LoggingFactory.Create`kullanın.</span><span class="sxs-lookup"><span data-stu-id="2f6b9-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="2f6b9-111">2,1 örnek:</span><span class="sxs-lookup"><span data-stu-id="2f6b9-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="2f6b9-112">3,0 örnek:</span><span class="sxs-lookup"><span data-stu-id="2f6b9-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="2f6b9-113">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2f6b9-113">Additional resources</span></span>

<xref:fundamentals/logging/index>