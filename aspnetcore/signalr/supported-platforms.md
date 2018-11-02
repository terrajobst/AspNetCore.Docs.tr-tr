---
title: ASP.NET Core SignalR tarafından desteklenen platformlar
author: tdykstra
description: ASP.NET Core SignalR için desteklenen platformlar ile ilgili öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758186"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR tarafından desteklenen platformlar

## <a name="server-system-requirements"></a>Sunucu sistem gereksinimleri

SignalR için ASP.NET Core, ASP.NET Core destekleyen herhangi bir sunucu platformu destekler.

## <a name="javascript-client"></a>JavaScript istemcisi

[JavaScript istemci](https://www.npmjs.com/package/@aspnet/signalr) NodeJS 8 ve sonraki sürümleri ve aşağıdaki tarayıcılardan çalışır:

| Tarayıcı                         | Sürüm |
| ------------------------------- | ------- |
| Microsoft Edge                  | geçerli |
| Mozilla Firefox                 | geçerli |
| Google Chrome; Android içerir | geçerli |
| Safari; iOS içerir            | geçerli |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>.NET istemci

[.NET istemci](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ASP.NET Core tarafından desteklenen herhangi bir sunucu platformu üzerinde çalışır.

IIS sunucusu çalıştırıyorsa, IIS 8.0 veya Windows Server 2012'de daha yüksek veya daha yüksek WebSockets aktarım gerektirir. Diğer aktarımları tüm platformlarda desteklenir.

## <a name="java-client"></a>Java istemci

[Java istemci](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 ve sonraki sürümleri destekler.

## <a name="unsupported-clients"></a>Desteklenmeyen İstemcileri

Aşağıdaki istemciler kullanılabilir ancak Deneysel ya da terim ve kısaltmalarla. Bunlar, şu anda desteklenmez ve hiçbir zaman olabilir.

* [C++ istemci](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [SWIFT istemci](https://github.com/moozzyk/SignalR-Client-Swift)
