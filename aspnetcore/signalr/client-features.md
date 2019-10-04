---
title: SignalR istemci özellikleri
author: bradygaster
description: Çeşitli ASP.NET Core SignalR istemcileri tarafından desteklenen özellikleri öğrenin.
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 6718722cdbcfae500026fcd429ca6b9de8f0f103
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925350"
---
# <a name="aspnet-core-signalr-client-features"></a>ASP.NET Core SignalR istemci özellikleri

## <a name="feature-distribution"></a>Özellik dağıtımı

Aşağıdaki tabloda, gerçek zamanlı destek sunan istemciler için özellikler ve destek gösterilmektedir. Her özellik için, bu özelliği destekleyen *En düşük* sürüm listelenir. Listelenen bir sürüm yoksa özellik desteklenmez.

| Özellik | .NET | JavaScript | Java |
| ---- | :-: | :-: | :-: |
| Azure SignalR hizmeti desteği |1.0.0|1.0.0|1.0.0|
| [Sunucudan istemciye akış](xref:signalr/streaming)          |1.0.0|1.0.0|1.0.0|
| [İstemciden sunucuya akış](xref:signalr/streaming)          |3.0.0|3.0.0|3.0.0|
| Otomatik yeniden bağlanma ([.net](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))          |3.0.0|3.0.0|❌|
| WebSockets taşıma |1.0.0|1.0.0|1.0.0|
| Sunucu tarafından gönderilen olay aktarımı |1.0.0|1.0.0|❌|
| Uzun yoklama taşıması |1.0.0|1.0.0|3.0.0|
| JSON hub 'ı Protokolü |1.0.0|1.0.0|1.0.0|
| MessagePack Hub Protokolü |1.0.0|1.0.0|❌|

Java istemcisinde otomatik yeniden bağlanma desteği, [sorun izleyici](https://github.com/aspnet/AspNetCore/issues/8711)'de izlenir.

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core için SignalR ile çalışmaya başlama](xref:tutorials/signalr)
* [Desteklenen platformlar](xref:signalr/supported-platforms)
* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
