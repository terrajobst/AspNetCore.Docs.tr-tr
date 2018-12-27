---
title: ASP.NET Core signalr'a giriş
author: tdykstra
description: Uygulamalar için gerçek zamanlı işlevsellik eklemeye ASP.NET Core SignalR kitaplığı nasıl kolaylaştırdığını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: da18837c690d2182589db5f486ae74e537ade931
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425152"
---
# <a name="introduction-to-aspnet-core-signalr"></a>ASP.NET Core signalr'a giriş

## <a name="what-is-signalr"></a>SignalR nedir?

ASP.NET Core SignalR, uygulamalara gerçek zamanlı web işlevselliği ekleme basitleştiren bir açık kaynak kitaplığıdır. Gerçek zamanlı web işlevselliği, sunucu tarafı kodu anında içeriği istemcilere anında sağlar.

SignalR için iyi adaylar:

* Yüksek sıklık düzeyi güncelleştirmeleri sunucudan gerektiren uygulamaları. Oyunlar, sosyal ağlar, oylama, artırma, haritalar ve GPS uygulamaları verilebilir.
* Panoları ve izleme uygulamaları. Örnek şirket panoları, anında satış güncelleştirmeleri içerir ya da seyahat uyarıları.
* İşbirliğine dayalı uygulamalar. Beyaz Tahta uygulamaları ve yazılım toplantı takım işbirliğine dayalı uygulamalar örnekleridir.
* Bildirimleri gerektiren uygulamaları. Sosyal ağlar, e-posta, sohbet, oyunlar, seyahat uyarılarına ve diğer birçok uygulama bildirimleri kullanın.

SignalR server istemcisi oluşturmak için bir API sağlar [uzaktan yordam çağrısı (RPC)](https://wikipedia.org/wiki/Remote_procedure_call). RPC JavaScript işlevlerini sunucu tarafı .NET Core koddan istemcilerde çağırın.

ASP.NET Core için SignalR özelliklerinden bazıları şunlardır:

* Bağlantı Yönetimi otomatik olarak işler.
* İletileri eşzamanlı olarak bağlanan tüm istemciler için gönderir. Sohbet odası.
* Özel istemciler veya istemci gruplarının iletileri gönderir.
* Artan trafiği işlemeye ölçeklendirir.

Kaynak barındırılan bir [GitHub deposunu SignalR](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).

## <a name="transports"></a>Taşımalar

SignalR, gerçek zamanlı iletişimler işlemek için çeşitli teknikler destekler:

* [WebSockets](https://tools.ietf.org/html/rfc7118)
* Sunucu tarafından gönderilen olayları
* Uzun yoklama

SignalR istemci ve sunucu kapasitesini en iyi aktarım yöntemi otomatik olarak seçer.

## <a name="hubs"></a>Hub'ları

SignalR kullanan *hubs* istemciler ve sunucular arasında iletişim kurmak için.

Bir hub'ı istemci ve sunucu birbirleri üzerinde yöntemleri çağırmak izin veren bir üst düzey bir işlem hattı ' dir. SignalR sunucusunda ve yöntemlerini çağırmak istemcilerin otomatik olarak makine sınırlarında gönderme işler. Model bağlama sağlayan yöntemleri için parametre türü kesin belirlenmiş geçirebilirsiniz. SignalR sağlayan iki yerleşik hub protokol: bir metin protokolü temel JSON ve temel bir ikili Protokolü [MessagePack](https://msgpack.org/).  MessagePack genellikle JSON ile karşılaştırıldığında daha küçük iletileri oluşturur. Eski tarayıcılar desteklemelidir [XHR Düzey 2](https://caniuse.com/#feat=xhr2) MessagePack protokolü desteği sağlamak için.

Hub adı ve istemci tarafı yönteminin parametreleri içeren iletiler göndererek istemci-tarafı kodu çağırın. Yapılandırılmış protokolü kullanarak yöntem parametreleri olarak gönderilen nesneleri seri. İstemci, istemci tarafı kod içinde bir yönteme adıyla eşleşecek şekilde çalışır. İstemci bir eşleşme bulduğunda yöntemini çağırır ve seri durumdan çıkarılmış parametre verileri geçirir.

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core için SignalR ile çalışmaya başlama](xref:tutorials/signalr)
* [Desteklenen Platformlar](xref:signalr/supported-platforms)
* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
