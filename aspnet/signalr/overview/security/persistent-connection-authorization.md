---
uid: signalr/overview/security/persistent-connection-authorization
title: "Kimlik doğrulama ve yetkilendirme için SignalR kalıcı bağlantılar | Microsoft Docs"
author: pfletcher
description: "Bu konu, kalıcı bir bağlantı üzerindeki yetkilendirmeyi açıklar. Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9c6fff86ae6b1b65e6ba9922b6b8448643ef1f15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Kimlik doğrulama ve yetkilendirme için SignalR kalıcı bağlantılar
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)

> Bu konu, kalıcı bir bağlantı üzerindeki yetkilendirmeyi açıklar. Bir SignalR uygulamasına güvenlik tümleştirme hakkında genel bilgi için bkz: [güvenlik giriş](introduction-to-security.md). 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Bu konuda kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Bu konunun önceki sürümleri
> 
> SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


## <a name="enforce-authorization"></a>Yetkilendirmeyi

Kullanırken yetkilendirme kurallarını zorunlu tutmak için bir [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) geçersiz kılmanız gerekir `AuthorizeRequest` yöntemi. Kullanamazsınız `Authorize` özniteliği ile kalıcı bağlantılar. `AuthorizeRequest` Yöntemi, kullanıcının istenen eylemi gerçekleştirmek için yetkili olup olmadığını doğrulamak için her istek önce SignalR Framework tarafından çağrılır. `AuthorizeRequest` Yöntemi istemci tarafından çağrılmaz; bunun yerine, uygulamanızın standart kimlik doğrulama mekanizması kullanıcı kimliğini.

Aşağıdaki örnek, kimliği doğrulanmış kullanıcıların isteklerine sınırlamak gösterilmiştir.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Herhangi bir özelleştirilmiş yetkilendirme mantık AuthorizeRequest yönteminde ekleyebilirsiniz; gibi bir kullanıcının belirli bir role ait olup olmadığı denetleniyor.
