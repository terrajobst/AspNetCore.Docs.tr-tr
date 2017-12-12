---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "SignalR bağlantıları için SignalR kullanıcıları eşlemek 1.x | Microsoft Docs"
author: pfletcher
description: "Bu konu, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri korumak nasıl gösterir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 561c5739c4e8465efeb4b5d1eaf8a196dab8673f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>SignalR bağlantıları için SignalR kullanıcıları eşlemek 1.x
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)

> Bu konu, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri korumak nasıl gösterir.


## <a name="introduction"></a>Giriş

Bir hub'a bağlanan her istemci benzersiz bağlantı kimliği geçirir. Bu değeri alabilir `Context.ConnectionId` hub bağlamını özelliği. Uygulamanız için bağlantı kimliği kullanıcısıyla ve bu eşlemenin kalıcı hale getirmek gerekirse, aşağıdakilerden birini kullanabilirsiniz:

- [Bellek içi depolama](#inmemory), bir sözlük gibi
- [Her kullanıcı için SignalR grubu](#groups)
- [Kalıcı ve dış depolama](#database)gibi bir veritabanı tablosu veya Azure tablo depolaması

Bu uygulamaların her biri, bu konu başlığı altında gösterilir. Kullandığınız `OnConnected`, `OnDisconnected`, ve `OnReconnected` yöntemlerinin `Hub` kullanıcı bağlantı durumunu izlemek için sınıf.

Uygulamanız için en iyi yaklaşımı bağlıdır:

- Uygulamanızı barındıran web sunucularının sayısı.
- Bağlı durumda kullanıcıların listesini almak gerekmediğini.
- Olup uygulama veya sunucu yeniden başlatıldığında grup ve kullanıcı bilgileri kalıcı olması gerekir.
- Bir dış sunucu çağırma gecikme süresi bir sorun olup olmadığı.

Bu noktalar için hangi yaklaşımın çalışır aşağıdaki tabloda gösterilmektedir.

|  | Birden çok sunucu | Şu anda bağlı kullanıcıların listesini al | Bilgileri yeniden başlatıldıktan sonra Sürdür | En iyi performans |
| --- | --- | --- | --- | --- |
| Bellek içi |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Tek kullanıcı grupları | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Kalıcı, dış | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Bellek içi depolama

Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgileri bellekte depolanır sözlükte tutulacak gösterilmektedir. Sözlük kullanan bir `HashSet` bağlantı kimliği depolamak için. Herhangi bir anda bir kullanıcı birden fazla bağlantı SignalR uygulamaya sahip olabilir. Örneğin, birden çok aygıt ya da birden fazla tarayıcı sekmesi üzerinden bağlı bir kullanıcı birden fazla bağlantı kimliği gerekir.

Uygulama kapanırsa, tüm bilgileri kaybolur, ancak kullanıcıların kendi bağlantılarını yeniden oluşturmak gibi yeniden doldurulur. Ortamınızda birden fazla web sunucusu varsa her sunucu bağlantıları ayrı koleksiyonu gerekir çünkü bellek içi depolama çalışmaz.

İlk örnek bağlantıları kullanıcılara eşleme yöneten bir sınıfı gösterir. Anahtar Hashset'i için kullanıcının adı olacaktır.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Sonraki örnek bir hub'dan bağlantı eşleme sınıfının nasıl kullanılacağını gösterir. Sınıfının örneğini bir değişken adı depolanan `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Tek kullanıcı grupları

Her kullanıcı için bir grup oluşturun ve yalnızca o kullanıcıyı erişmek istediğinizde bu gruba ileti gönderme. Her grubun adı, kullanıcı adıdır. Bir kullanıcının birden çok bağlantı varsa, her bağlantı kimliği kullanıcı grubuna eklenir.

Kullanıcı kestiğinde, el ile kullanıcı grubundan kaldırmamanız. Bu eylem SignalR çerçevesi tarafından otomatik olarak gerçekleştirilir.

Aşağıdaki örnekte nasıl tek kullanıcı gruplarına uygulanacağını gösterir.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Kalıcı ve dış depolama

Bu konuda nasıl bağlantı bilgilerini depolamak için bir veritabanı veya Azure tablo depolaması kullanılacağını gösterir. Bu yaklaşım, her web sunucusu ile aynı veri deposu etkileşim kurabilen olduğundan, birden çok web sunucusu olduğunda çalışır. Web sunucuları çalışma veya uygulama yeniden başlatılmadan durdurursanız `OnDisconnected` yöntemi çağrılmaz. Bu nedenle, veri deponuz artık geçerli olmayan bağlantı kimlikleri için kaydeder olduğundan mümkündür. Yalnız bırakılmış kayıtlarının temizlemek için uygulamanız için uygun bir zaman çerçevesi dışında oluşturulmuş herhangi bir bağlantısı geçersiz kılmak isteyebilir. Bağlantı oluşturulduğunda, izleme için bir değer bu bölümdeki örnek verilebilir, ancak arka plan işlemi olarak yapmak isteyebilirsiniz çünkü eski kayıtları temizlemek nasıl gösterme.

### <a name="database"></a>Veritabanı

Aşağıdaki örnekler bir veritabanı bağlantısı ve kullanıcı bilgileri korumak nasıl gösterir. Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte Entity Framework kullanarak modelleri tanımlamak nasıl gösterir. Bu varlık modelleri veritabanı tabloları ve alanları karşılık gelir. Veri yapısını uygulamanızın gereksinimlerine bağlı olarak önemli ölçüde farklılık.

İlk örnek birçok bağlantı varlıkları ile ilişkilendirilebilir bir kullanıcı varlığı tanımlamak nasıl gösterir.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Ardından, hub'ından aşağıda kod ile her bağlantının durumunu izleyebilirsiniz.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Azure tablo depolaması

Aşağıdaki Azure tablo depolama örnek veritabanı örnektekine benzer. Tüm Azure tablo depolama hizmeti ile çalışmaya başlamak için gereken bilgileri içermez. Bilgi için bkz: [.NET tablo depolamasından kullanmayı](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).

Aşağıdaki örnekte, bağlantı bilgilerini depolamak için bir tablo varlığı gösterir. Kullanıcı adına göre verileri bölümler ve bir kullanıcı birden çok bağlantı herhangi bir zamanda çalışabilmeniz için her bir varlık bağlantı kimliğine göre tanımlar.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

Hub'ı, her kullanıcının bağlantısının durumunu izler.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
