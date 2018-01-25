---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "SignalR kullanıcıları eşlemek için bağlantıları | Microsoft Docs"
author: tfitzmac
description: "Bu konu, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri korumak nasıl gösterir. Bu konuda yazma CAN Fletcher'dan Yardım. Bu konuda kullanılan yazılım sürümleri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a>Eşleme SignalR bağlantıları kullanıcılara
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu konu, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri korumak nasıl gösterir.
> 
> Bu konuda yazma CAN Fletcher'dan Yardım.
> 
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


## <a name="introduction"></a>Giriş

Bir hub'a bağlanan her istemci benzersiz bağlantı kimliği geçirir. Bu değeri alabilir `Context.ConnectionId` hub bağlamını özelliği. Uygulamanız için bağlantı kimliği kullanıcısıyla ve bu eşlemenin kalıcı hale getirmek gerekirse, aşağıdakilerden birini kullanabilirsiniz:

- [Kullanıcı kimlik sağlayıcısı (SignalR 2)](#IUserIdProvider)
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
| UserId sağlayıcısı | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| Bellek içi |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Tek kullanıcı grupları | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Kalıcı, dış | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>IUserID sağlayıcısı

Bu özellik, USERID belirtmek kullanıcılara bir Irequest IUserIdProvider yeni arabirimi aracılığıyla bağlı.

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Varsayılan olarak, olacaktır kullanıcının kullandığı uygulaması `IPrincipal.Identity.Name` kullanıcı adı. Bunu değiştirmek için uygulamanızı kaydetmek `IUserIdProvider` uygulamanız başladığında genel ana bilgisayar ile:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Gelen bir hub içinde bu kullanıcılara aşağıdaki API aracılığıyla iletileri göndermek kullanabileceksiniz:

**Belirli bir kullanıcı için bir ileti gönderme**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Bellek içi depolama

Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgileri bellekte depolanır sözlükte tutulacak gösterilmektedir. Sözlük kullanan bir `HashSet` bağlantı kimliği depolamak için. Herhangi bir anda bir kullanıcı birden fazla bağlantı SignalR uygulamaya sahip olabilir. Örneğin, birden çok aygıt ya da birden fazla tarayıcı sekmesi üzerinden bağlı bir kullanıcı birden fazla bağlantı kimliği gerekir.

Uygulama kapanırsa, tüm bilgileri kaybolur, ancak kullanıcıların kendi bağlantılarını yeniden oluşturmak gibi yeniden doldurulur. Ortamınızda birden fazla web sunucusu varsa her sunucu bağlantıları ayrı koleksiyonu gerekir çünkü bellek içi depolama çalışmaz.

İlk örnek bağlantıları kullanıcılara eşleme yöneten bir sınıfı gösterir. Anahtar Hashset'i için kullanıcının adı olacaktır.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Sonraki örnek bir hub'dan bağlantı eşleme sınıfının nasıl kullanılacağını gösterir. Sınıfının örneğini bir değişken adı depolanan `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Tek kullanıcı grupları

Her kullanıcı için bir grup oluşturun ve yalnızca o kullanıcıyı erişmek istediğinizde bu gruba ileti gönderme. Her grubun adı, kullanıcı adıdır. Bir kullanıcının birden çok bağlantı varsa, her bağlantı kimliği kullanıcı grubuna eklenir.

Kullanıcı kestiğinde, el ile kullanıcı grubundan kaldırmamanız. Bu eylem SignalR çerçevesi tarafından otomatik olarak gerçekleştirilir.

Aşağıdaki örnekte nasıl tek kullanıcı gruplarına uygulanacağını gösterir.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Kalıcı ve dış depolama

Bu konuda nasıl bağlantı bilgilerini depolamak için bir veritabanı veya Azure tablo depolaması kullanılacağını gösterir. Bu yaklaşım, her web sunucusu ile aynı veri deposu etkileşim kurabilen olduğundan, birden çok web sunucusu olduğunda çalışır. Web sunucuları çalışma veya uygulama yeniden başlatılmadan durdurursanız `OnDisconnected` yöntemi çağrılmaz. Bu nedenle, veri deponuz artık geçerli olmayan bağlantı kimlikleri için kaydeder olduğundan mümkündür. Yalnız bırakılmış kayıtlarının temizlemek için uygulamanız için uygun bir zaman çerçevesi dışında oluşturulmuş herhangi bir bağlantısı geçersiz kılmak isteyebilir. Bağlantı oluşturulduğunda, izleme için bir değer bu bölümdeki örnek verilebilir, ancak arka plan işlemi olarak yapmak isteyebilirsiniz çünkü eski kayıtları temizlemek nasıl gösterme.

### <a name="database"></a>Veritabanı

Aşağıdaki örnekler bir veritabanı bağlantısı ve kullanıcı bilgileri korumak nasıl gösterir. Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte Entity Framework kullanarak modelleri tanımlamak nasıl gösterir. Bu varlık modelleri veritabanı tabloları ve alanları karşılık gelir. Veri yapısını uygulamanızın gereksinimlerine bağlı olarak önemli ölçüde farklılık.

İlk örnek birçok bağlantı varlıkları ile ilişkilendirilebilir bir kullanıcı varlığı tanımlamak nasıl gösterir.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Ardından, hub'ından aşağıda kod ile her bağlantının durumunu izleyebilirsiniz.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Azure tablo depolaması

Aşağıdaki Azure tablo depolama örnek veritabanı örnektekine benzer. Tüm Azure tablo depolama hizmeti ile çalışmaya başlamak için gereken bilgileri içermez. Bilgi için bkz: [.NET tablo depolamasından kullanmayı](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Aşağıdaki örnekte, bağlantı bilgilerini depolamak için bir tablo varlığı gösterir. Kullanıcı adına göre verileri bölümler ve bir kullanıcı birden çok bağlantı herhangi bir zamanda çalışabilmeniz için her bir varlık bağlantı kimliğine göre tanımlar.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Hub'ı, her kullanıcının bağlantısının durumunu izler.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
