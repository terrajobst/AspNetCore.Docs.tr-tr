---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: SignalR bağlantıları için SignalR kullanıcılarını eşleme 1.x | Microsoft Docs
author: pfletcher
description: Bu konuda, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri saklamanın gösterilmektedir.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: ba7bbea25cd35f8ba23901c5f8db08404c91bd52
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287494"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>SignalR 1.x Sürümünde SignalR Kullanıcılarını Bağlantılarla Eşleme
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Bu konuda, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri saklamanın gösterilmektedir.


## <a name="introduction"></a>Giriş

Bir hub'a bağlanan her istemci benzersiz bağlantı kimliği geçirir. Bu değeri alabilirsiniz `Context.ConnectionId` hub bağlamını özelliğidir. Uygulamanız için bağlantı kimliği bir kullanıcı eşleme ve bu eşlemenin kalıcı hale getirmek gerekiyorsa, şunlardan birini kullanabilirsiniz:

- [Bellek içi depolama](#inmemory), bir sözlük gibi
- [Her kullanıcı için SignalR grubu](#groups)
- [Kalıcı, dış depolama](#database)bir veritabanı tablosu veya Azure tablo depolama gibi

Bu uygulamalardan her biri, bu konudaki gösterilir. Kullandığınız `OnConnected`, `OnDisconnected`, ve `OnReconnected` yöntemlerinin `Hub` kullanıcı bağlantı durumunu izlemek için sınıf.

Uygulamanız için en iyi yaklaşım bağlıdır:

- Uygulamanızı barındıran web sunucularının sayısı.
- Olup şu anda bağlı olan kullanıcıların listesini almanız gerekir.
- Uygulama veya sunucu yeniden başlatıldığında, Grup ve kullanıcı bilgilerini kalıcı hale gerekip gerekmediğini.
- Bir dış sunucu arama gecikme süresi bir sorun olup olmadığı.

Hangi yaklaşımın çalıştığı için bu konuları aşağıdaki tabloda gösterilmektedir.

|  | Birden fazla sunucu | Şu anda bağlı olan kullanıcıların listesini alma | Yeniden başlatıldıktan sonra bilgilerini kalıcı hale | En iyi performans |
| --- | --- | --- | --- | --- |
| Bellek içi |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Tek kullanıcı grupları | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Kalıcı, dış | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Bellek içi depolama

Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgileri bellekte depolanır bir sözlükte tutulacak gösterilmektedir. Sözlük kullanan bir `HashSet` bağlantı kimliği depolamak için. Herhangi bir zamanda bir kullanıcı birden fazla bağlantı SignalR uygulamaya sahip olabilir. Örneğin, birden çok cihaz ya da birden fazla tarayıcı sekmesinde bağlı bir kullanıcı birden fazla bağlantı kimliği gerekir.

Uygulama kapanır, tüm bilgileri kaybolur, ancak kullanıcıların kendi bağlantılarını yeniden oluşturma gibi yeniden doldurulur. Ortamınızda birden fazla web sunucusu varsa her sunucuyu ayrı bir bağlantı koleksiyonu yeterli olacağından, bellek içi depolama çalışmaz.

İlk örnek bağlantılar için kullanıcı eşleme yöneten bir sınıfı gösterir. HashSet anahtarı kullanıcı adı olacaktır.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Sonraki örnek, bir hub'ından bağlantı eşleme sınıfının nasıl kullanılacağını gösterir. Sınıfının örneğini, bir değişken adı ile depolanan `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Tek kullanıcı grupları

Her kullanıcı için bir grup oluşturun ve yalnızca bu kullanıcının erişmek istediğinizde bu gruba bir ileti gönderin. Her grubun adı, kullanıcı adıdır. Bir kullanıcının birden fazla bağlantı varsa, her bağlantı kimliği kullanıcı grubuna eklenir.

Kullanıcı kesildiğinde, el ile kullanıcı grubundan kaldırmamanız gerekir. Bu eylem, SignalR framework tarafından otomatik olarak gerçekleştirilir.

Aşağıdaki örnek, tek kullanıcı gruplarına uygulanacağını gösterilmektedir.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Kalıcı, dış depolama

Bu konuda, bağlantı bilgilerini depolamak için bir veritabanı veya Azure tablo depolama kullanma gösterilmektedir. Bu yaklaşım, her web sunucusunun aynı veri deposu ile etkileşim kurabilir çünkü birden çok web sunucusu olduğunda çalışır. Web sunucularınız çalışma ya da uygulama yeniden başlatılmadan durdurursanız `OnDisconnected` yöntemi çağrılmadı. Bu nedenle, veri deponuz artık geçerli olmayan bir bağlantı kimlikleri için kayıtları olacağını mümkündür. Artık bu kayıtları temizlemek için uygulamanız için uygun bir zaman çerçevesi dışında oluşturulmuş herhangi bir bağlantı geçersiz kılmak isteyebilirsiniz. Bu bölümdeki örneklerde bağlantıyı oluştururken izlemek için bir değer içerir ancak arka plan işlemi olarak bunu isteyebilirsiniz, çünkü eski kayıtları temizlemek nasıl gösterme.

### <a name="database"></a>Veritabanı

Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgilerini bir veritabanında saklamak gösterilmektedir. Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte, Entity Framework kullanarak modelleri tanımlamak nasıl gösterir. Bu varlık modeli, veritabanı tabloları ve alanları karşılık gelir. Data yapınız, uygulama gereksinimlerine bağlı olarak önemli ölçüde değişiklik gösterebilir.

İlk örnek, birçok bağlantı varlıklar ile ilişkilendirilebilir bir kullanıcı varlığı tanımlamak gösterilmektedir.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Ardından, hub'ından aşağıda kod ile her bağlantının durumunu izleyebilirsiniz.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Azure tablo depolama

Aşağıdaki Azure tablo depolama örnek veritabanı örneğe benzerdir. Tüm Azure tablo depolama hizmeti ile kullanmaya başlamak için gereken bilgileri içermez. Bilgi için [tablo Depolama'yı .NET kullanma](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Aşağıdaki örnek, bağlantı bilgilerini depolamak için bir tablo varlığı gösterir. Kullanıcı adına göre verileri bölümler ve kullanıcının herhangi bir zamanda birden çok bağlantı sahip olabileceği için bağlantı kimliği tarafından her varlık tanımlar.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

Hub'ında her kullanıcının bağlantının durumunu izleyin.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
