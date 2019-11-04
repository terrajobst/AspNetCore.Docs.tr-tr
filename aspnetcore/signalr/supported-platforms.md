---
title: ASP.NET Core SignalR desteklenen platformlar
author: bradygaster
description: ASP.NET Core SignalR için desteklenen platformlar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2019
uid: signalr/supported-platforms
ms.openlocfilehash: 1be7a307710e6e522c0088fd1ca01da11a13eda1
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426975"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR desteklenen platformlar

## <a name="server-system-requirements"></a>Sunucu sistem gereksinimleri

ASP.NET Core için SignalR, ASP.NET Core desteklediği tüm sunucu platformunu destekler.

## <a name="javascript-client"></a>JavaScript istemcisi

[JavaScript Istemcisi](https://www.npmjs.com/package/@aspnet/signalr) NodeJS 8 ve sonraki sürümlerde ve aşağıdaki tarayıcılarda çalışır:

| Tarayıcı                         | Version         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | Geçerli&dagger; |
| Mozilla Firefox                 | Geçerli&dagger; |
| Google Chrome; Android içerir | Geçerli&dagger; |
| Uygulamasını iOS içerir            | Geçerli&dagger; |
| Microsoft Internet Explorer     | 11              |

&dagger;*geçerli* , tarayıcının en son sürümünü ifade eder.

## <a name="net-client"></a>.NET istemcisi

[.Net Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) , ASP.NET Core tarafından desteklenen herhangi bir platformda çalışır. Örneğin, Xamarin geliştiricileri, Xamarin. iOS 8.4.0.1 ve üzeri ile iOS uygulamalarını ve Xamarin. iOS 11.14.0.4 ve üstünü kullanarak Android uygulamaları oluşturmak için [SignalR 'yi kullanabilir](https://github.com/aspnet/Announcements/issues/305) .

Sunucu IIS çalıştırıyorsa, WebSockets aktarımı Windows Server 2012 veya üzeri sürümlerde IIS 8,0 veya sonraki bir sürümü gerektirir. Diğer aktarımlar tüm platformlarda desteklenir.

## <a name="java-client"></a>Java istemcisi

[Java istemcisi](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) , Java 8 ve sonraki sürümlerini destekler.

## <a name="unsupported-clients"></a>Desteklenmeyen istemciler

Aşağıdaki istemciler mevcuttur, ancak deneysel veya resmi olmayan bir. Şu anda desteklenmemektedir ve hiçbir şekilde bulunmayabilir.

* [C++istemcilerinin](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Swift istemcisi](https://github.com/moozzyk/SignalR-Client-Swift)
