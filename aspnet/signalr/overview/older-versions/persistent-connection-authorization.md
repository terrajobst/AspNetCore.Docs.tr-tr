---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Kimlik doğrulama ve yetkilendirme için SignalR kalıcı bağlantılar (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Bu konu, kalıcı bir bağlantı üzerindeki yetkilendirmeyi açıklar. Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036108"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Kimlik doğrulama ve yetkilendirme için SignalR kalıcı bağlantılar (SignalR 1.x)
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)

> Bu konu, kalıcı bir bağlantı üzerindeki yetkilendirmeyi açıklar. Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için bkz: [güvenlik giriş](index.md).


## <a name="enforce-authorization"></a>Yetkilendirmeyi

Kullanırken yetkilendirme kurallarını zorunlu tutmak için bir [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) geçersiz kılmanız gerekir `AuthorizeRequest` yöntemi. Kullanamazsınız `Authorize` özniteliği ile kalıcı bağlantılar. `AuthorizeRequest` Yöntemi, kullanıcının istenen eylemi gerçekleştirmek için yetkili olup olmadığını doğrulamak için her istek önce SignalR Framework tarafından çağrılır. `AuthorizeRequest` Yöntemi istemci tarafından çağrılmaz; bunun yerine, uygulamanızın standart kimlik doğrulama mekanizması kullanıcı kimliğini.

Aşağıdaki örnek, kimliği doğrulanmış kullanıcıların isteklerine sınırlamak gösterilmiştir.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Herhangi bir özelleştirilmiş yetkilendirme mantık AuthorizeRequest yönteminde ekleyebilirsiniz; gibi bir kullanıcının belirli bir role ait olup olmadığı denetleniyor.
