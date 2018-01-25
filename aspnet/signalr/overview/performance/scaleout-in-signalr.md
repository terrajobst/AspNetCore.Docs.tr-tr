---
uid: signalr/overview/performance/scaleout-in-signalr
title: "SignalR genişletme giriş | Microsoft Docs"
author: MikeWasson
description: "Yazılım sürümleri bu konuda Visual Studio 2013 .NET 4.5 SignalR önceki sürümleri hakkında bilgi için bu konuda sürüm 2 önceki sürümlerinde kullanılan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: f1d15250682305f6d0512b72bd2e40cb4a8a18e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-scaleout-in-signalr"></a>SignalR genişletme giriş
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)

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


Genel olarak, bir web uygulaması ölçeklendirmek için iki yolu vardır: *ölçeği* ve *ölçeğini*.

- Daha büyük bir sunucuya (veya daha büyük bir VM) kullanarak daha fazla RAM, CPU'lar, vb. ile anlamına gelir ölçeklendirin.
- Yükü işlemek üzere daha fazla sunucu anlamına gelir ölçeklendirin.

Yükseltme ile hızlı bir şekilde bir makine boyutu sınırı isabet sorunudur. Bunun ötesinde ölçeğini gerekir. Ölçeği genişletme, ancak, istemciler farklı sunuculara yönlendirilmiş. Bir sunucuya bağlı bir istemci, başka bir sunucudan gönderilen iletileri almaz.

![](scaleout-in-signalr/_static/image1.png)

Bir çözümdür iletilerini adlı bir bileşen kullanarak sunucuları arasında iletmek için bir *devre kartı*. Etkin bir devre kartı ile her uygulama örneği devre kartına ileti gönderir ve bunları diğer uygulama örnekleri devre kartı iletir. (Elektronik bir devre kartı paralel bağlayıcılar grubudur. Benzerleme tarafından SignalR devre kartı birden çok sunucuya bağlanır.)

![](scaleout-in-signalr/_static/image2.png)

SignalR şu anda üç backplanes sağlar:

- **Azure Service Bus**. Hizmet veri yolu geniş bağlı şekilde iletileri göndermek bileşenleri izin veren bir ileti altyapısıdır.
- **Redis**. Redis bir bellek içi anahtar-değer deposudur. Redis ileti göndermek için bir yayımlama/abonelik ("pub/alt") desen destekler.
- **SQL Server**. SQL Server devre kartına ileti SQL tablolara yazar. Devre kartına verimli ileti için hizmet Aracısı kullanır. Service Broker etkin değilse ancak, bu da çalışır.

Uygulamanızı Azure üzerinde dağıtırsanız, Redis devre kartı kullanarak göz önünde bulundurun [Azure Redis önbelleği](https://azure.microsoft.com/services/cache/). Kendi sunucu grubuna dağıtıyorsanız, SQL Server veya Redis backplanes göz önünde bulundurun.

Aşağıdaki konular her devre kartı yönelik adım adım öğreticiler içerir:

- [Azure Service Bus ile SignalR Ölçeğini Genişletme](scaleout-with-windows-azure-service-bus.md)
- [Redis ile SignalR Ölçeğini Genişletme](scaleout-with-redis.md)
- [SQL Server ile SignalR Ölçeğini Genişletme](scaleout-with-sql-server.md)

## <a name="implementation"></a>Uygulama

SignalR öğesinde her ileti bir ileti veri yolu gönderilir. Bir ileti veri yoluna uygulayan [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) Yayımla ve abone bir Özet sağlar arabirimi. Varsayılan değiştirerek backplanes iş **IMessageBus** o devre kartı için tasarlanmış bir veri yolu ile. Örneğin, Redis için ileti veri yolu olan [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), ve Redis kullanan [pub/alt](http://redis.io/topics/pubsub) ileti gönderme ve alma için bir mekanizma.

Her sunucu örneği devre kartı veri yolu üzerinden bağlanır. Bir ileti gönderildiğinde devre kartına gider ve isteğe bağlı olarak devre kartı her sunucuya gönderir. Bir sunucu kartından bir ileti aldığında, iletiyi yerel önbelleğinde koyar. Sunucu, bunun yerel önbelleğinden istemcilere iletileri sonra sunar.

Her istemci bağlantısı için bir imleç kullanarak istemcinin sürüyor ileti akışı okuma izlenir. (Bir imleç ileti akışı içinde bir konumu temsil eder.) Bir istemci bağlantısını keser ve sonra yeniden bağlandığında, istemcinin imleç değerinden sonra gelen iletileri için veri yolu sorar. Bir bağlantı kullandığında aynı şey [uzun yoklama](../getting-started/introduction-to-signalr.md#transports). Bir uzun yoklama istek tamamlandıktan sonra istemci yeni bir bağlantı açar ve imleci sonra gelen iletilerinin ister.

Bir istemci üzerinde farklı bir sunucuya yönlendirilir olsa bile yeniden bağlantı imleç mekanizması çalışır. Devre kartına tüm sunucular farkındadır ve bir istemcinin bağlandığı hangi sunucunun önemli değildir.

## <a name="limitations"></a>Sınırlamalar

Bir devre kartı kullanarak, en fazla ileti işleme istemcileri doğrudan tek bir sunucu düğüme konuşurken olduğundan daha düşüktür. Devre kartına bir performans sorunu olabilmesi için her düğüm için her ileti devre kartı iletir olmasıdır. Bu sınırlama bir sorun olup olmadığını uygulamaya bağlıdır. Örneğin, bazı tipik SignalR senaryolar şunlardır:

- [Sunucu yayın](../getting-started/tutorial-server-broadcast-with-signalr.md) (örn., borsa): Backplanes iletileri gönderilir oranı sunucu denetimleri olduğundan bu senaryo için iyi çalışır.
- [İstemci istemci](../getting-started/tutorial-getting-started-with-signalr.md) (örn., sohbet): istemci sayısı ileti sayısını ölçeklendirir Bu senaryoda devre kartı bir performans sorunu olabilir; diğer bir deyişle, iletiler oranını büyürse orantılı olarak daha fazla istemci katılma.
- [Yüksek yoğunlukta gerçek zamanlı](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (örneğin, gerçek zamanlı oyun): bir devre kartı bu senaryo için önerilmez.

## <a name="enabling-tracing-for-signalr-scaleout"></a>SignalR genişletme için izlemeyi etkinleştirme

Backplanes izlemeyi etkinleştirmek için aşağıdaki bölümleri web.config dosyasını kökü altındaki eklemek **yapılandırma** öğe:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
