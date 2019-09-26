---
title: SignalR istemci özellikleri
author: bradygaster
description: Çeşitli ASP.NET Core SignalR istemcileri tarafından desteklenen özellikleri öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301185"
---
# <a name="aspnet-core-signalr-client-features"></a>ASP.NET Core SignalR istemci özellikleri

## <a name="feature-distribution"></a>Özellik dağıtımı

Aşağıdaki tabloda, gerçek zamanlı destek sunan istemciler için özellikler ve destek gösterilmektedir.

| Özellik | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Azure SignalR hizmeti desteği |✔|✔|✔|
| [Sunucudan istemciye akış](xref:signalr/streaming)          |✔|✔|✔|
| [İstemciden sunucuya akış](xref:signalr/streaming)          |✔|✔|✔|
| Otomatik yeniden bağlanma ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |✔|✔| |
| WebSockets taşıma |✔|✔|✔|
| Sunucu tarafından gönderilen olay aktarımı |✔|✔| |
| Uzun yoklama taşıması |✔|✔|✔|
| JSON hub 'ı Protokolü |✔|✔|✔|
| MessagePack Hub Protokolü |✔|✔| |

Java istemcisinde otomatik yeniden bağlanma desteği, [sorun izleyici](https://github.com/aspnet/AspNetCore/issues/8711)'de izlenir.

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core için SignalR ile çalışmaya başlama](xref:tutorials/signalr)
* [Desteklenen platformlar](xref:signalr/supported-platforms)
* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
