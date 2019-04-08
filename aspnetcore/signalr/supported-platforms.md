---
title: ASP.NET Core SignalR tarafından desteklenen platformlar
author: bradygaster
description: ASP.NET Core SignalR için desteklenen platformlar ile ilgili öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/06/2019
uid: signalr/supported-platforms
ms.openlocfilehash: fefaaf97de3f1fabf8f3154bf5b4ccb37195ccff
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068228"
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
 
## <a name="net-client"></a>.NET istemcisi

[.NET istemci](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) ASP.NET Core tarafından desteklenen tüm platformlarda çalışır. Örneğin, [Xamarin geliştiricilerine SignalR kullanabileceğiniz](https://github.com/aspnet/Announcements/issues/305) Xamarin.Android 8.4.0.1 kullanarak Android uygulamaları oluşturmak ve daha sonra ve Xamarin.iOS 11.14.0.4 kullanarak iOS uygulamaları ve daha sonra.

IIS sunucusu çalıştırıyorsa, WebSockets taşıma IIS 8.0 veya üzeri, Windows Server 2012 veya üzerini gerektirir. Diğer aktarımları tüm platformlarda desteklenir.

## <a name="java-client"></a>Java istemcisi

[Java istemci](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) Java 8 ve sonraki sürümleri destekler.

## <a name="unsupported-clients"></a>Desteklenmeyen İstemcileri

Aşağıdaki istemciler kullanılabilir ancak Deneysel ya da terim ve kısaltmalarla. Bunlar, şu anda desteklenmez ve hiçbir zaman olabilir.

* [C++ istemci](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [SWIFT istemci](https://github.com/moozzyk/SignalR-Client-Swift)
