---
title: ASP.NET Core SignalR giriş
author: rachelappel
description: Uygulamalar için gerçek zamanlı işlevsellik ekleme ASP.NET Core SignalR kitaplığı nasıl basitleştirir öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: 190dfe9eac95be646b458870ac4ee95f681f45d7
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a>ASP.NET Core SignalR giriş

Tarafından [Rachel Appel](https://twitter.com/rachelappel)


[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-signalr"></a>SignalR nedir?

ASP.NET Core SignalR uygulamalarını ekleme gerçek zamanlı web işlevselliği basitleştiren bir kitaplıktır. Gerçek zamanlı web işlevselliği sunucu tarafı kodu anında içeriği istemcilere hemen sağlar.

SignalR için iyi adaylar:

* Yüksek yoğunlukta güncelleştirmeleri sunucudan gerektiren uygulamalar. Örnek oyunlara, sosyal ağlara, oylama, artırma, eşlemeleri ve GPS uygulamaları verilebilir.
* Panolar ve izleme uygulamaları. Örnek şirket panoları, anlık satış güncelleştirmeleri dahil veya uyarıları seyahat.
* İşbirlikçi uygulamalar. Beyaz Tahta uygulamaları ve yazılım toplantı takım işbirliği uygulamalara örnektir.
* Bildirimleri gerektiren uygulamalar. Sosyal ağlar, e-posta, sohbet, oyunlar, seyahat uyarıları ve diğer birçok uygulama bildirimleri kullanın.

SignalR server istemcisi oluşturmak için bir API sağlar [uzaktan yordam çağrısı (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). RPC'ler JavaScript işlevlerini sunucu tarafı .NET Core koddan istemcilerde çağırın.

SignalR ASP.NET Core için:

* Bağlantı Yönetimi otomatik olarak yönetir.
* İletileri aynı anda bağlanan tüm istemciler için yayın etkinleştirir. Sohbet yer.
* Belirli istemciler veya istemci gruplarının ileti gönderilmesini sağlar.
* Açık kaynaklıdır konumunda olduğundan [GitHub](https://github.com/aspnet/signalr).
* Ölçeklenebilir.

Bir HTTP bağlantısı farklı olarak istemci ve sunucu arasındaki bağlantının kalıcıdır.

## <a name="transports"></a>Taşımalar

Gerçek zamanlı web uygulamaları oluşturmak için teknikleri sayısı üzerinden SignalR özetleri. [WebSockets](https://tools.ietf.org/html/rfc7118) en iyi Aktarım, ancak bunlar kullanılamaz Server-Sent olayları ve uzun yoklama gibi başka teknikler kullanılabilir. SignalR otomatik olarak algılar ve sunucu ve istemci üzerinde desteklenen özelliklere bağlı olarak uygun bir taşıma başlatılamıyor.

## <a name="hubs"></a>Hub'ları

SignalR hub'ları istemcileri ve sunucuları arasında iletişim kurmak için kullanır.

Bir hub istemci ve sunucu birbirine yöntemlerini çağırmaya izin veren üst düzey bir işlem hattı ' dir. SignalR makine sınırları içinde otomatik olarak göndermeyi kolayca yerel yöntemleri ve tersi olarak sunucuda yöntemlerini çağırmaya istemcilere izin verme işler. Hub yöntemleri için kesin tür belirtilmiş parametreleri geçirme model bağlama sağlayan izin verir. SignalR iki yerleşik hub protokol sağlar: bir metin protokolü temel JSON ve temel bir ikili protokol [MessagePack](https://msgpack.org/).  MessagePack genellikle JSON kullanırken daha küçük ileti oluşturur. Eski tarayıcılar desteklemelidir [XHR Düzey 2](https://caniuse.com/#feat=xhr2) MessagePack protokolü desteği sağlamak için.

Hub etkin taşımanın kullanarak iletiler göndererek istemci-tarafı kodu çağırma. İletileri adını ve istemci tarafı yönteminin parametrelerini içerir. Nesneleri yöntem parametreleri, yapılandırılmış protokolü kullanılarak seri durumdan olarak gönderilir. İstemci, istemci tarafı kodlar yönteminde adına eşleştirmeyi dener. Bir eşleşme gerçekleştiğinde, istemci yöntemi seri durumdan çıkarılmış parametre veri kullanarak çalışır.

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core için SignalR ile çalışmaya başlama](xref:signalr/get-started)
* [Desteklenen Platformlar](xref:signalr/supported-platforms)
* [Hub'ları](xref:signalr/hubs)
* [JavaScript istemci](xref:signalr/javascript-client)
