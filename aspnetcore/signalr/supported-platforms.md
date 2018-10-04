---
title: ASP.NET Core SignalR tarafından desteklenen platformlar
author: tdykstra
description: ASP.NET Core SignalR için desteklenen platformlar
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577632"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR tarafından desteklenen platformlar

## <a name="server-system-requirements"></a>Sunucu sistem gereksinimleri

SignalR için ASP.NET Core, ASP.NET Core destekleyen herhangi bir sunucu platformu destekler.

## <a name="javascript-client"></a>JavaScript istemcisi

[JavaScript istemci](https://www.npmjs.com/package/@aspnet/signalr) NodeJS 8 ve sonraki sürümleri ve aşağıdaki tarayıcılardan çalışır:

| Tarayıcı | Sürüm |
| ------- | ------- |
| Microsoft Edge | geçerli |
| Mozilla Firefox | geçerli |
| Google Chrome; Android içerir | geçerli |
| Safari; iOS içerir | geçerli |
| Microsoft Internet Explorer | 11 |
 
## <a name="net-client"></a>.NET istemci

[.NET istemci](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ASP.NET Core tarafından desteklenen herhangi bir sunucu platformu üzerinde çalışır.

Server IIS çalıştığında, Windows Server 2012 veya üzeri sürümde WebSockets taşıma IIS 8.0 veya sonraki bir sürümünü gerektirir. Diğer aktarımları tüm platformlarda desteklenir.

## <a name="java-client"></a>Java istemci

[Java istemci](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 ve sonraki sürümleri destekler.

## <a name="unsupported-clients"></a>Desteklenmeyen İstemcileri

Aşağıdaki istemciler kullanılabilir ancak Deneysel ya da terim ve kısaltmalarla. Bunlar artık desteklenmez ve her zamankinden desteklenmiyor olabilir.

* [C++ istemci](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [SWIFT istemci](https://github.com/moozzyk/SignalR-Client-Swift)
