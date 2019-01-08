---
title: 2.1 için 2.2 veya 3.0 Microsoft.Extensions.Logging geçiş
author: pakrym
description: 2.2 veya 3.0 Microsoft.Extensions.Logging 2.1 kullanan bir ASP.NET Core uygulaması geçirmeyi öğrenin.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099538"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="ad839-103">2.1 için 2.2 veya 3.0 Microsoft.Extensions.Logging geçiş</span><span class="sxs-lookup"><span data-stu-id="ad839-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="ad839-104">Bu makalede kullanan ASP.NET Core uygulaması geçirmek için genel adımlar özetlenmektedir `Microsoft.Extensions.Logging` 2.1 için 2.2 veya 3. 0'dan.</span><span class="sxs-lookup"><span data-stu-id="ad839-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="ad839-105">2.1 için 2.2</span><span class="sxs-lookup"><span data-stu-id="ad839-105">2.1 to 2.2</span></span>

<span data-ttu-id="ad839-106">El ile oluşturmanız `ServiceCollection` ve çağrı `AddLogging`.</span><span class="sxs-lookup"><span data-stu-id="ad839-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="ad839-107">2.1 örnek:</span><span class="sxs-lookup"><span data-stu-id="ad839-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="ad839-108">2,2 örnek:</span><span class="sxs-lookup"><span data-stu-id="ad839-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="ad839-109">2.1-3.0</span><span class="sxs-lookup"><span data-stu-id="ad839-109">2.1 to 3.0</span></span>

<span data-ttu-id="ad839-110">3. 0'kullanın `LoggingFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="ad839-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="ad839-111">2.1 örnek:</span><span class="sxs-lookup"><span data-stu-id="ad839-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="ad839-112">3.0 Örnek:</span><span class="sxs-lookup"><span data-stu-id="ad839-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="ad839-113">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ad839-113">Additional resources</span></span>

<xref:fundamentals/logging/index>