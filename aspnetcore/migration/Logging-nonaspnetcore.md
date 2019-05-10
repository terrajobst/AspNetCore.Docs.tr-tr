---
title: 2.1 için 2.2 veya 3.0 Microsoft.Extensions.Logging geçiş
author: pakrym
description: 2.2 veya 3.0 Microsoft.Extensions.Logging 2.1 kullanan bir ASP.NET Core uygulaması geçirmeyi öğrenin.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898841"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>2.1 için 2.2 veya 3.0 Microsoft.Extensions.Logging geçiş

Bu makalede kullanan ASP.NET Core uygulaması geçirmek için genel adımlar özetlenmektedir `Microsoft.Extensions.Logging` 2.1 için 2.2 veya 3. 0'dan.

## <a name="21-to-22"></a>2.1’den 2.2’ye

El ile oluşturmanız `ServiceCollection` ve çağrı `AddLogging`.

2.1 örnek:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2,2 örnek:

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1-3.0

3. 0'kullanın `LoggingFactory.Create`.

2.1 örnek:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

3.0 Örnek:

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>Ek kaynaklar

<xref:fundamentals/logging/index>