---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: SignalR kullanıcılarını bağlantılarla eşleme | Microsoft Docs
author: tfitzmac
description: Bu konuda, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri saklamanın gösterilmektedir. Patrick Fletcher, bu konuda yazma yardımcı olmuştur. Bu konu başlığında kullanılan yazılım sürümleri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: bd7c0cd9a645ab5b65c5c1446b51ea1646e43799
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391538"
---
<a name="mapping-signalr-users-to-connections"></a>SignalR kullanıcılarını bağlantılarla eşleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu konuda, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri saklamanın gösterilmektedir.
> 
> Patrick Fletcher, bu konuda yazma yardımcı olmuştur.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Bu konu başlığında kullanılan yazılım sürümleri
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
> SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


## <a name="introduction"></a>Giriş

Bir hub'a bağlanan her istemci benzersiz bağlantı kimliği geçirir. Bu değeri alabilirsiniz `Context.ConnectionId` hub bağlamını özelliğidir. Uygulamanız için bağlantı kimliği bir kullanıcı eşleme ve bu eşlemenin kalıcı hale getirmek gerekiyorsa, şunlardan birini kullanabilirsiniz:

- [Kullanıcı kimlik sağlayıcısı (SignalR 2)](#IUserIdProvider)
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
| Sağlayıcı kullanıcı kimliği | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| Bellek içi |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Tek kullanıcı grupları | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Kalıcı, dış | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>IUserID sağlayıcısı

Bu özellik, USERID belirtmek kullanıcılara bir Irequest IUserIdProvider yeni arabirimi aracılığıyla temel.

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Varsayılan olarak, olacaktır kullanıcının kullanan bir uygulama `IPrincipal.Identity.Name` kullanıcı adı. Bunu değiştirmek için uygulamanız kaydetme `IUserIdProvider` , uygulamanız başlatıldığında genel konakla:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Bir hub içinde gelen bu kullanıcılara aşağıdaki API'si üzerinden ileti göndermek mümkün olacaktır:

**Belirli bir kullanıcıya bir ileti gönderme**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Bellek içi depolama

Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgileri bellekte depolanır bir sözlükte tutulacak gösterilmektedir. Sözlük kullanan bir `HashSet` bağlantı kimliği depolamak için. Herhangi bir zamanda bir kullanıcı birden fazla bağlantı SignalR uygulamaya sahip olabilir. Örneğin, birden çok cihaz ya da birden fazla tarayıcı sekmesinde bağlı bir kullanıcı birden fazla bağlantı kimliği gerekir.

Uygulama kapanır, tüm bilgileri kaybolur, ancak kullanıcıların kendi bağlantılarını yeniden oluşturma gibi yeniden doldurulur. Ortamınızda birden fazla web sunucusu varsa her sunucuyu ayrı bir bağlantı koleksiyonu yeterli olacağından, bellek içi depolama çalışmaz.

İlk örnek bağlantılar için kullanıcı eşleme yöneten bir sınıfı gösterir. HashSet anahtarı kullanıcı adı olacaktır.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Sonraki örnek, bir hub'ından bağlantı eşleme sınıfının nasıl kullanılacağını gösterir. Sınıfının örneğini, bir değişken adı ile depolanan `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Tek kullanıcı grupları

Her kullanıcı için bir grup oluşturun ve yalnızca bu kullanıcının erişmek istediğinizde bu gruba bir ileti gönderin. Her grubun adı, kullanıcı adıdır. Bir kullanıcının birden fazla bağlantı varsa, her bağlantı kimliği kullanıcı grubuna eklenir.

Kullanıcı kesildiğinde, el ile kullanıcı grubundan kaldırmamanız gerekir. Bu eylem, SignalR framework tarafından otomatik olarak gerçekleştirilir.

Aşağıdaki örnek, tek kullanıcı gruplarına uygulanacağını gösterilmektedir.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Kalıcı, dış depolama

Bu konuda, bağlantı bilgilerini depolamak için bir veritabanı veya Azure tablo depolama kullanma gösterilmektedir. Bu yaklaşım, her web sunucusunun aynı veri deposu ile etkileşim kurabilir çünkü birden çok web sunucusu olduğunda çalışır. Web sunucularınız çalışma ya da uygulama yeniden başlatılmadan durdurursanız `OnDisconnected` yöntemi çağrılmadı. Bu nedenle, veri deponuz artık geçerli olmayan bir bağlantı kimlikleri için kayıtları olacağını mümkündür. Artık bu kayıtları temizlemek için uygulamanız için uygun bir zaman çerçevesi dışında oluşturulmuş herhangi bir bağlantı geçersiz kılmak isteyebilirsiniz. Bu bölümdeki örneklerde bağlantıyı oluştururken izlemek için bir değer içerir ancak arka plan işlemi olarak bunu isteyebilirsiniz, çünkü eski kayıtları temizlemek nasıl gösterme.

### <a name="database"></a>Veritabanı

Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgilerini bir veritabanında saklamak gösterilmektedir. Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte, Entity Framework kullanarak modelleri tanımlamak nasıl gösterir. Bu varlık modeli, veritabanı tabloları ve alanları karşılık gelir. Data yapınız, uygulama gereksinimlerine bağlı olarak önemli ölçüde değişiklik gösterebilir.

İlk örnek, birçok bağlantı varlıklar ile ilişkilendirilebilir bir kullanıcı varlığı tanımlamak gösterilmektedir.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Ardından, hub'ından aşağıda kod ile her bağlantının durumunu izleyebilirsiniz.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Azure tablo depolama

Aşağıdaki Azure tablo depolama örnek veritabanı örneğe benzerdir. Tüm Azure tablo depolama hizmeti ile kullanmaya başlamak için gereken bilgileri içermez. Bilgi için [tablo Depolama'yı .NET kullanma](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Aşağıdaki örnek, bağlantı bilgilerini depolamak için bir tablo varlığı gösterir. Kullanıcı adına göre verileri bölümler ve kullanıcının herhangi bir zamanda birden çok bağlantı sahip olabileceği için bağlantı kimliği tarafından her varlık tanımlar.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Hub'ında her kullanıcının bağlantının durumunu izleyin.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
