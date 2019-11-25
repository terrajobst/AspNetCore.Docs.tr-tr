---
title: Desteklenen SignalR platformları ASP.NET Core
author: bradygaster
description: ASP.NET Core SignalRiçin desteklenen platformlar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 9b9cf1d57d61c333c485f23b7ab952c66814d2aa
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317474"
---
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a>Desteklenen SignalR platformları ASP.NET Core

## <a name="server-system-requirements"></a>Sunucu sistem gereksinimleri

ASP.NET Core için SignalR, ASP.NET Core desteklediği tüm sunucu platformunu destekler.

## <a name="javascript-client"></a>JavaScript istemcisi

[JavaScript Istemcisi](xref:signalr/javascript-client) NodeJS 8 ve sonraki sürümlerde ve aşağıdaki tarayıcılarda çalışır:

| Browser                         | Sürüm         |
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

* [C++istemcilerinin](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Swift istemcisi](https://github.com/moozzyk/SignalR-Client-Swift)
