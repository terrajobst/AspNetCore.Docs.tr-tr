---
title: ASP.NET Core SignalR tarafından desteklenen platformlar
author: tdykstra
description: ASP.NET Core SignalR için desteklenen platformlar ile ilgili öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/12/2018
uid: signalr/supported-platforms
ms.openlocfilehash: be3d4d0049395fb2499bd0b4aac126e953ce7910
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861725"
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

[.NET istemci](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ASP.NET Core tarafından desteklenen tüm platformlarda çalışır. Örneğin, [Xamarin geliştiricilerine SignalR kullanabileceğiniz](https://github.com/aspnet/Announcements/issues/305) Xamarin.Android 8.4.0.1 kullanarak Android uygulamaları oluşturmak ve daha sonra ve Xamarin.iOS 11.14.0.4 kullanarak iOS uygulamaları ve daha sonra.

IIS sunucusu çalıştırıyorsa, IIS 8.0 veya Windows Server 2012'de daha yüksek veya daha yüksek WebSockets aktarım gerektirir. Diğer aktarımları tüm platformlarda desteklenir.

## <a name="java-client"></a>Java istemci

[Java istemci](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 ve sonraki sürümleri destekler.

## <a name="unsupported-clients"></a>Desteklenmeyen İstemcileri

Aşağıdaki istemciler kullanılabilir ancak Deneysel ya da terim ve kısaltmalarla. Bunlar, şu anda desteklenmez ve hiçbir zaman olabilir.

* [C++ istemci](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [SWIFT istemci](https://github.com/moozzyk/SignalR-Client-Swift)
