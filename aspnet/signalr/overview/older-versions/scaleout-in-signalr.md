---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: Signalr'da ölçek genişletmeye giriş 1.x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: a492eabfb5a74472b44095f24704028f6cd8a12a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375227"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>Signalr'da ölçek genişletmeye giriş 1.x
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

Genel olarak, bir web uygulamasını ölçeklendirmek için iki yol vardır: *ölçeği* ve *ölçeğini*.

- Ölçeğini daha büyük bir sunucuya (veya daha büyük bir VM) kullanarak daha fazla RAM, CPU, vb. ile anlamına gelir.
- Yükü işlemek için daha fazla sunucu ekleyerek anlamına gelir ölçeklendirin.

Büyütme ile hızlı bir şekilde bir makine boyutu sınırı isabet bir sorundur. Bunun ötesinde ölçeklendirme gerekir. Ancak, ölçeği genişlettiğinizde, istemcilerin farklı sunuculara yönlendirilir. Bir sunucuya bağlı bir istemci, başka bir sunucudan gönderilen iletileri almazsınız.

![](scaleout-in-signalr/_static/image1.png)

Bir çözüm ise adlı bir bileşen kullanma, sunucular arasında iletileri iletecek şekilde bir *devre kartı*. Etkin bir devre kartı ile her uygulama örneği devre kartına ileti gönderir ve devre kartına bunları diğer uygulama örnekleri iletir. (Elektronik bir devre kartı paralel bağlayıcılar grubudur. Benzerleme tarafından SignalR devre kartı birden çok sunucuya bağlanır.)

![](scaleout-in-signalr/_static/image2.png)

SignalR, şu anda üç backplanes sağlar:

- **Azure Service Bus**. Service Bus iletileri gevşek bir şekilde göndermek bileşenler sağlayan bir Mesajlaşma altyapısıdır.
- **Redis**. Redis bellek içi anahtar-değer deposudur. Redis ileti göndermek için bir yayımlama/abone olma ("pub/sub") deseni destekler.
- **SQL Server**. SQL Server devre kartına ileti SQL tablolarının yazar. Devre kartına verimli Mesajlaşma için hizmet Aracısı kullanır. Hizmet Aracısı etkin değilse ancak, bu da çalışır.

Uygulamanızı azure'da dağıtın, Azure Service Bus devre kartına kullanmayı düşünün. Kendi sunucu grubuna dağıtıyorsanız, SQL Server veya Redis backplanes göz önünde bulundurun.

Aşağıdaki konular, her devre kartı yönelik adım adım öğreticiler içerir:

- [Azure Service Bus ile SignalR Ölçeğini Genişletme](scaleout-with-windows-azure-service-bus.md)
- [Redis ile SignalR Ölçeğini Genişletme](scaleout-with-redis.md)
- [SQL Server ile SignalR Ölçeğini Genişletme](scaleout-with-sql-server.md)

## <a name="implementation"></a>Uygulama

SignalR öğesinde her ileti bir ileti veri yolu gönderilir. Bir ileti veri yoluna uygulayan [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) arabirimi, yayımlama/abone olma bir Özet sağlar. Varsayılan değiştirerek backplanes çalışma **IMessageBus** o devre kartı için tasarlanan veri. Örneğin, Redis için ileti veri yolu olan [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), ve Redis kullanır [pub/sub](http://redis.io/topics/pubsub) ileti göndermek ve almak için mekanizma.

Her sunucu örneği devre kartına Veriyolu aracılığıyla bağlanır. Bir ileti gönderildiğinde devre kartına gider ve isteğe bağlı olarak devre kartına her sunucuya gönderir. Bir sunucu devre kartından bir ileti aldığında, iletiyi yerel önbelleğinde koyar. Sunucu, bunun yerel önbelleğinden istemciler için iletileri ardından sunar.

Her istemci bağlantısı için bir işaretçi kullanarak ileti akışı okuma istemcinin ilerleme izlenir. (İleti akışı bir konumda bir imleç gösterir.) Bir istemci bağlantısını keser ve daha sonra yeniden bağlanacağı, istemcinin imleç değerinden sonra gelen iletileri için veri yolu sorar. Bir bağlantı kullanırken aynı şey [uzun yoklama](../getting-started/introduction-to-signalr.md#transports). Uzun yoklama istek tamamlandıktan sonra istemci yeni bir bağlantı açar ve imleci sonra gelen iletileri ister.

Bir istemci üzerinde farklı bir sunucuya yönlendirilmesini bile yeniden bağlantı imleç mekanizması çalışır. Devre kartına tüm sunucuların farkındadır ve bir istemcinin bağlandığı hangi sunucunun önemi yoktur.

## <a name="limitations"></a>Sınırlamalar

Bir devre kartı kullanarak, en fazla ileti işleme hızı istemcileri doğrudan tek bir sunucu düğüme konuşurken olduğundan daha küçük. Devre kartına bir performans sorunu olabilmesi için her düğüme her ileti devre kartı iletir olmasıdır. Bu sınırlama bir sorun olup olmadığını, uygulamaya bağlıdır. Örneğin, bazı tipik SignalR senaryoları şunlardır:

- [Sunucu yayın](tutorial-server-broadcast-with-aspnet-signalr.md) (örneğin, bandı): Backplanes sunucu iletileri gönderilir oranı denetlediğinden bu senaryo için iyi çalışır.
- [İstemci istemci](tutorial-getting-started-with-signalr.md) (örneğin, sohbet edin): ileti sayısını ölçeklendirir istemci sayısı, bu senaryoda, bir performans sorunu devre kartına olabilir; diğer bir deyişle, orantılı olarak daha fazla istemciye iletileri oranı büyürse katılın.
- [Yüksek sıklıkta gerçek zamanlı](tutorial-high-frequency-realtime-with-signalr.md) (örneğin, gerçek zamanlı oyun): Bu senaryo için bir devre kartı önerilmez.

## <a name="enabling-tracing-for-signalr-scaleout"></a>SignalR ölçeğini genişletme için izlemeyi etkinleştirme

Backplanes için izlemeyi etkinleştirmek için web.config dosyasının kök altında aşağıdaki bölümlerde ekleme **yapılandırma** öğesi:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
