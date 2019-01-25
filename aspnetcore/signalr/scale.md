---
title: ASP.NET Core SignalR üretim barındırma ve ölçeklendirme
author: bradygaster
description: Performans ve ASP.NET Core SignalR kullanan uygulamalarda sorunları ölçeklendirme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
ms.openlocfilehash: 4ac4509acc89d0091a3757c7cfbc9981614f29ad
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836928"
---
# <a name="aspnet-core-signalr-hosting-and-scaling"></a>ASP.NET Core SignalR barındırma ve ölçeklendirme

Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), ve [Tom Dykstra](https://github.com/tdykstra),

Bu makalede, barındırma ve ölçeklendirme ASP.NET Core SignalR kullanan yüksek trafik uygulamaları konuları açıklanmaktadır.

## <a name="tcp-connection-resources"></a>TCP bağlantı kaynakları

Bir web sunucusu destekleyebileceği eş zamanlı TCP bağlantısı sayısı sınırlıdır. Standart HTTP istemciler *kısa ömürlü* bağlantıları. Bu bağlantılar, istemci boşta gider ve daha sonra yeniden açılmasını kapatılabilir. Öte yandan, SignalR bir bağlantıdır *kalıcı*. SignalR bağlantıları bile istemci boşta gittiğinde açık kalır. Birden çok istemci hizmet veren bir yüksek trafik uygulamasında bu kalıcı bağlantılar, bağlantı sayısı üst sınırı isabet sunucuları neden olabilir.

Kalıcı bağlantılar, ayrıca her bağlantı izlemek için bazı ek bellek tüketir.

Aynı sunucuda barındırılan diğer web apps ile SignalR bağlantısı ile ilgili kaynakların aşırı kullanımı etkileyebilir. SignalR açar ve son kullanılabilir TCP bağlantıları tutar, aynı sunucuda diğer web uygulamalarını de kullanabilecekleri daha fazla bağlantı var.

Bir sunucu bağlantılar dışında çalıştırıyorsa, rastgele yuva hatalar görürsünüz ve bağlantı hataları sıfırlayın. Örneğin:

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

Diğer web uygulamalarında hata neden SignalR kaynak kullanımı tutmak için SignalR diğer web uygulamalarınızı daha farklı sunucularda çalıştırın.

Bir SignalR uygulamasında hataları neden SignalR kaynak kullanımı tutmak için bir sunucu işlemek zorundadır bağlantı sayısını sınırlamak için ölçeği.

## <a name="scale-out"></a>Ölçeklendirme

Bir sunucu grubu için sorunları oluşturan kendi bağlantılarını izlemek SignalR kullanan bir uygulamayı gerekir. Bir sunucu eklemek ve diğer sunuculara bilgi sahibi olmadığınız yeni bağlantıları alır. Örneğin, aşağıdaki diyagramda her bir sunucuda SignalR bağlantıları diğer sunucularda farkında değil. Tüm istemciler için bir ileti göndermek SignalR sunuculardan biri üzerinde istediği zaman, iletiyi yalnızca sunucuya bağlı istemcileri gider.

![Bir devre kartı SignalR ölçeklendirme](scale/_static/scale-no-backplane.png)

Bu sorunu çözmek için Seçenekler [Azure SignalR hizmeti](#azure-signalr-service) ve [devre kartı olarak Redis](#redis-backplane).

## <a name="azure-signalr-service"></a>Azure SignalR Hizmeti

Azure SignalR hizmeti, bir devre kartı yerine bir proxy ' dir. Her zaman bir istemci, sunucunun bir bağlantı başlatır, hizmete bağlanmak için istemci yönlendirilir. Bu işlem, aşağıdaki diyagramda gösterilmiştir:

![Azure SignalR hizmeti bağlantı kurma](scale/_static/azure-signalr-service-one-connection.png)

Aşağıdaki diyagramda gösterildiği gibi küçük bir bağlantı hizmetine sabit sayısı her sunucu gereksinimlerini hizmet tüm istemci bağlantıları yönetir, sonucudur:

![Hizmete bağlı istemciler hizmete bağlı sunucuları](scale/_static/azure-signalr-service-multiple-connections.png)

Bu yaklaşım, Ölçek genişletme için Redis devre kartı alternatif üzerinden çeşitli avantajları vardır:

* Olarak da bilinen, Yapışkan oturumlar [istemci benzeşimi](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), bağlandıklarında istemciler için Azure SignalR hizmeti hemen yönlendirilirsiniz çünkü gerekli değildir.
* Uygulama genişletebilir bir SignalR bağlantıları herhangi bir sayıda işlemek için otomatik olarak Azure SignalR hizmeti eşitlenene kadar gönderilen iletilerin sayısına göre. Örneğin, binlerce istemciden olabilir, ancak yalnızca birkaç ileti saniye başına gönderilen, yalnızca bağlantıları işlemek için birden çok sunucuya ölçeğini genişletmek SignalR uygulama gerekmez.
* Bir SignalR uygulama SignalR olmadan bir web uygulaması daha önemli ölçüde daha fazla bağlantı kaynaklarını kullanmaz.

Bu nedenlerle, App Service, VM'ler ve kapsayıcılar dahil olmak üzere, Azure üzerinde barındırılan tüm ASP.NET Core SignalR uygulamalar için Azure SignalR hizmeti öneririz.

Daha fazla bilgi için [Azure SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-overview).

## <a name="redis-backplane"></a>Redis devre kartı

[Redis](https://redis.io/) bir Mesajlaşma sistemi ile bir yayımlama/abone olma modelini destekleyen bir bellek içi anahtar-değer deposudur. SignalR Redis devre kartına ileti başka bir sunucuya iletmek için pub/sub özelliğini kullanır. Bir istemci bir bağlantı kurar, bağlantı bilgilerini devre kartına geçirilir. Tüm istemciler için bir ileti göndermek bir sunucu istediğinde devre kartına gönderir. Devre kartına tüm bağlı istemcileri ve hangi bilir sunucuları oldukları üzerinde. Tüm istemciler kendi ilgili sunucuları aracılığıyla ileti gönderir. Bu işlem, aşağıdaki diyagramda gösterilmiştir:

![Redis devre kartı, bir sunucudan tüm istemcilere gönderilen ileti](scale/_static/redis-backplane.png)

Redis devre kartına kendi altyapınızda barındırılan uygulamalar için önerilen genişleme yaklaşımdır. Azure SignalR hizmeti ile şirket içi uygulamalar, veri merkezi ve Azure veri merkezi arasında bağlantı gecikme nedeniyle üretim kullanımı için pratik bir seçenek değildir.

Azure SignalR hizmeti sağladığı daha önce not ettiğiniz Redis devre kartına dezavantajları şunlardır:

* Olarak da bilinen, Yapışkan oturumlar [istemci benzeşimi](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), gereklidir. Bir bağlantı, bir sunucu üzerinde başlatıldıktan sonra o sunucuda kalmak bağlantı vardır.
* Birkaç iletilerinin gönderilme olsa bile bir SignalR uygulaması istemcilerin sayısına göre Ölçeklendirmesi gerekir.
* Bir SignalR uygulaması önemli ölçüde daha fazla bağlantı kaynak SignalR olmadan bir web uygulaması kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-overview)
* [Bir Redis devre kartı ayarlayın](xref:signalr/redis-backplane)
