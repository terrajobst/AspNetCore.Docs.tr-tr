---
title: Desteklenen SignalR platformları ASP.NET Core
author: bradygaster
description: ASP.NET Core SignalRiçin desteklenen platformlar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 054965921c87c1a9be27e5ddaa8a87b0fa1f4113
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78668142"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR desteklenen platformlar

## <a name="server-system-requirements"></a>Sunucu sistem gereksinimleri

ASP.NET Core için SignalR, ASP.NET Core desteklediği tüm sunucu platformunu destekler.

## <a name="javascript-client"></a>JavaScript istemcisi

[JavaScript Istemcisi](xref:signalr/javascript-client) NodeJS 8 ve sonraki sürümlerde ve aşağıdaki tarayıcılarda çalışır:

| Tarayıcı                         | Sürüm         |
| ------------------------------- | --------------- |
| Microsoft Edge                  | Geçerli&dagger; |
| Mozilla Firefox                 | Geçerli&dagger; |
| Google Chrome; Android içerir | Geçerli&dagger; |
| Uygulamasını iOS içerir            | Geçerli&dagger; |
| Microsoft Internet Explorer     | 11              |

&dagger;*geçerli* , tarayıcının en son sürümünü ifade eder.

## <a name="net-client"></a>.NET istemcisi

[.Net Client](xref:signalr/dotnet-client) , ASP.NET Core tarafından desteklenen herhangi bir platformda çalışır. Örneğin, Xamarin geliştiricileri, Xamarin. iOS 8.4.0.1 ve üstünü kullanarak Android uygulamaları oluşturmak için [SignalRkullanabilir](https://github.com/aspnet/Announcements/issues/305) ve Xamarin. iOS 11.14.0.4 ve üstünü kullanarak iOS uygulamaları oluşturabilir.

Sunucu IIS çalıştırıyorsa, WebSockets aktarımı Windows Server 2012 veya üzeri sürümlerde IIS 8,0 veya sonraki bir sürümü gerektirir. Diğer aktarımlar tüm platformlarda desteklenir.

## <a name="java-client"></a>Java istemcisi

[Java istemcisi](xref:signalr/java-client) , Java 8 ve sonraki sürümlerini destekler.

## <a name="unsupported-clients"></a>Desteklenmeyen istemciler

Aşağıdaki istemciler mevcuttur, ancak deneysel veya resmi olmayan bir. Şu anda desteklenmemektedir ve hiçbir şekilde bulunmayabilir.

* [C++istemcilerinin](https://github.com/aspnet/SignalR-Client-Cpp)

* [Swift istemcisi](https://github.com/moozzyk/SignalR-Client-Swift)
