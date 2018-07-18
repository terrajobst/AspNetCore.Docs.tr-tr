---
title: ASP.NET Core signalr'a giriş
author: tdykstra
description: Uygulamalar için gerçek zamanlı işlevsellik eklemeye ASP.NET Core SignalR kitaplığı nasıl kolaylaştırdığını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095395"
---
# <a name="introduction-to-aspnet-core-signalr"></a>ASP.NET Core signalr'a giriş

Tarafından [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>SignalR nedir?

ASP.NET Core SignalR uygulamalarına gerçek zamanlı web işlevselliği ekleme basitleştiren bir kitaplıktır. Gerçek zamanlı web işlevselliği, sunucu tarafı kodu anında içeriği istemcilere anında sağlar.

SignalR için iyi adaylar:

* Yüksek sıklık düzeyi güncelleştirmeleri sunucudan gerektiren uygulamaları. Oyunlar, sosyal ağlar, oylama, artırma, haritalar ve GPS uygulamaları verilebilir.
* Panoları ve izleme uygulamaları. Örnek şirket panoları, anında satış güncelleştirmeleri içerir ya da seyahat uyarıları.
* İşbirliğine dayalı uygulamalar. Beyaz Tahta uygulamaları ve yazılım toplantı takım işbirliğine dayalı uygulamalar örnekleridir.
* Bildirimleri gerektiren uygulamaları. Sosyal ağlar, e-posta, sohbet, oyunlar, seyahat uyarılarına ve diğer birçok uygulama bildirimleri kullanın.

SignalR server istemcisi oluşturmak için bir API sağlar [uzaktan yordam çağrısı (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). RPC JavaScript işlevlerini sunucu tarafı .NET Core koddan istemcilerde çağırın.

ASP.NET Core için SignalR:

* Bağlantı Yönetimi otomatik olarak işler.
* İletileri eşzamanlı olarak bağlanan tüm istemciler için yayın etkinleştirir. Sohbet odası.
* Özel istemciler veya istemci gruplarının ileti gönderilmesini sağlar.
* Açık kaynak haline getirildi adresindeki [GitHub](https://github.com/aspnet/signalr).
* Ölçeklenebilir.

Bir HTTP bağlantısı farklı olarak istemci ve sunucu arasındaki bağlantıyı kalıcıdır.

## <a name="transports"></a>Taşımalar

Gerçek zamanlı web uygulamaları oluşturmaya yönelik teknikleri sayısı üzerinden SignalR özetleri. [WebSockets](https://tools.ietf.org/html/rfc7118) en iyi Aktarım, ancak bu kullanılamayan Server-Sent olayları ve uzun yoklama gibi başka teknikler kullanılabilir. SignalR otomatik olarak algılar ve sunucu ve istemci desteklenen özelliklere bağlı olarak uygun bir taşıma başlatılamıyor.

## <a name="hubs"></a>Hub'ları

SignalR hub'ları, istemciler ve sunucular arasında iletişim kurmak için kullanır.

Bir hub'ı, istemci ve sunucu birbirleri üzerinde yöntemleri çağırmak izin veren bir üst düzey bir işlem hattı ' dir. SignalR istemcilerinin yerel yöntemler olarak kolayca ve tersi olarak, sunucu üzerinde yöntemleri çağırmak otomatik olarak makine sınırlarında gönderme işler. Hub yöntemleri için kesin türü belirtilmiş parametre geçirme model bağlama sağlayan izin verin. SignalR sağlayan iki yerleşik hub protokol: bir metin protokolü temel JSON ve temel bir ikili Protokolü [MessagePack](https://msgpack.org/).  MessagePack genellikle JSON kullanırken daha küçük ileti oluşturur. Eski tarayıcılar desteklemelidir [XHR Düzey 2](https://caniuse.com/#feat=xhr2) MessagePack protokolü desteği sağlamak için.

Hub'ları, istemci tarafı kod kullanarak etkin iletiler göndererek çağırın. İletiler, adı ve istemci tarafı yönteminin parametreleri içerir. Yapılandırılmış protokolü kullanarak yöntem parametreleri olarak gönderilen nesneleri seri. İstemci, istemci tarafı kod içinde bir yönteme adıyla eşleşecek şekilde çalışır. Bir eşleşme olduğunda, istemci yöntemi kullanılarak seri durumdan çıkarılmış parametre veri çalıştırır.

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core için SignalR ile çalışmaya başlama](xref:tutorials/signalr)
* [Desteklenen Platformlar](xref:signalr/supported-platforms)
* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
