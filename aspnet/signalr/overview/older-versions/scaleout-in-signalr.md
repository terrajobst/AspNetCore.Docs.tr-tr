---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: "SignalR genişletme giriş 1.x | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: e6230d4d65adb8c9a064545ad761898ca53562bf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>SignalR genişletme giriş 1.x
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)

Genel olarak, bir web uygulaması ölçeklendirmek için iki yolu vardır: *ölçeği* ve *ölçeğini*.

- Daha büyük bir sunucuya (veya daha büyük bir VM) kullanarak daha fazla RAM, CPU'lar, vb. ile anlamına gelir ölçeklendirin.
- Yükü işlemek üzere daha fazla sunucu anlamına gelir ölçeklendirin.

Yükseltme ile hızlı bir şekilde bir makine boyutu sınırı isabet sorunudur. Bunun ötesinde ölçeğini gerekir. Ölçeği genişletme, ancak, istemciler farklı sunuculara yönlendirilmiş. Bir sunucuya bağlı bir istemci, başka bir sunucudan gönderilen iletileri almaz.

![](scaleout-in-signalr/_static/image1.png)

Bir çözümdür iletilerini adlı bir bileşen kullanarak sunucuları arasında iletmek için bir *devre kartı*. Etkin bir devre kartı ile her uygulama örneği devre kartına ileti gönderir ve bunları diğer uygulama örnekleri devre kartı iletir. (Elektronik bir devre kartı paralel bağlayıcılar grubudur. Benzerleme tarafından SignalR devre kartı birden çok sunucuya bağlanır.)

![](scaleout-in-signalr/_static/image2.png)

SignalR şu anda üç backplanes sağlar:

- **Azure hizmet veri yolu**. Hizmet veri yolu geniş bağlı şekilde iletileri göndermek bileşenleri izin veren bir ileti altyapısıdır.
- **Redis**. Redis bir bellek içi anahtar-değer deposudur. Redis ileti göndermek için bir yayımlama/abonelik ("pub/alt") desen destekler.
- **SQL Server**. SQL Server devre kartına ileti SQL tablolara yazar. Devre kartına verimli ileti için hizmet Aracısı kullanır. Service Broker etkin değilse ancak, bu da çalışır.

Uygulamanızı Azure üzerinde dağıtırsanız, Azure Service Bus devre kartı kullanmayı düşünün. Kendi sunucu grubuna dağıtıyorsanız, SQL Server veya Redis backplanes göz önünde bulundurun.

Aşağıdaki konular her devre kartı yönelik adım adım öğreticiler içerir:

- [Azure Service Bus ile SignalR genişletme](scaleout-with-windows-azure-service-bus.md)
- [Redis ile SignalR genişletme](scaleout-with-redis.md)
- [SQL Server ile SignalR genişletme](scaleout-with-sql-server.md)

## <a name="implementation"></a>Uygulama

SignalR öğesinde her ileti bir ileti veri yolu gönderilir. Bir ileti veri yoluna uygulayan [IMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) Yayımla ve abone bir Özet sağlar arabirimi. Varsayılan değiştirerek backplanes iş **IMessageBus** o devre kartı için tasarlanmış bir veri yolu ile. Örneğin, Redis için ileti veri yolu olan [RedisMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), ve Redis kullanan [pub/alt](http://redis.io/topics/pubsub) ileti gönderme ve alma için bir mekanizma.

Her sunucu örneği devre kartı veri yolu üzerinden bağlanır. Bir ileti gönderildiğinde devre kartına gider ve isteğe bağlı olarak devre kartı her sunucuya gönderir. Bir sunucu kartından bir ileti aldığında, iletiyi yerel önbelleğinde koyar. Sunucu, bunun yerel önbelleğinden istemcilere iletileri sonra sunar.

Her istemci bağlantısı için bir imleç kullanarak istemcinin sürüyor ileti akışı okuma izlenir. (Bir imleç ileti akışı içinde bir konumu temsil eder.) Bir istemci bağlantısını keser ve sonra yeniden bağlandığında, istemcinin imleç değerinden sonra gelen iletileri için veri yolu sorar. Bir bağlantı kullandığında aynı şey [uzun yoklama](../getting-started/introduction-to-signalr.md#transports). Bir uzun yoklama istek tamamlandıktan sonra istemci yeni bir bağlantı açar ve imleci sonra gelen iletilerinin ister.

Bir istemci üzerinde farklı bir sunucuya yönlendirilir olsa bile yeniden bağlantı imleç mekanizması çalışır. Devre kartına tüm sunucular farkındadır ve bir istemcinin bağlandığı hangi sunucunun önemli değildir.

## <a name="limitations"></a>Sınırlamalar

Bir devre kartı kullanarak, en fazla ileti işleme istemcileri doğrudan tek bir sunucu düğüme konuşurken olduğundan daha düşüktür. Devre kartına bir performans sorunu olabilmesi için her düğüm için her ileti devre kartı iletir olmasıdır. Bu sınırlama bir sorun olup olmadığını uygulamaya bağlıdır. Örneğin, bazı tipik SignalR senaryolar şunlardır:

- [Sunucu yayın](tutorial-server-broadcast-with-aspnet-signalr.md) (örn., borsa): Backplanes iletileri gönderilir oranı sunucu denetimleri olduğundan bu senaryo için iyi çalışır.
- [İstemci istemci](tutorial-getting-started-with-signalr.md) (örn., sohbet): istemci sayısı ileti sayısını ölçeklendirir Bu senaryoda devre kartı bir performans sorunu olabilir; diğer bir deyişle, iletiler oranını büyürse orantılı olarak daha fazla istemci katılma.
- [Yüksek yoğunlukta gerçek zamanlı](tutorial-high-frequency-realtime-with-signalr.md) (örneğin, gerçek zamanlı oyun): bir devre kartı bu senaryo için önerilmez.

## <a name="enabling-tracing-for-signalr-scaleout"></a>SignalR genişletme için izlemeyi etkinleştirme

Backplanes izlemeyi etkinleştirmek için aşağıdaki bölümleri web.config dosyasını kökü altındaki eklemek **yapılandırma** öğe:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
