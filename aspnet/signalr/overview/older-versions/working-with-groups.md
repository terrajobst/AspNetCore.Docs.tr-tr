---
uid: signalr/overview/older-versions/working-with-groups
title: "SignalR grupları ile çalışma 1.x | Microsoft Docs"
author: pfletcher
description: "Bu konu, grup üyeliği bilgileri Hub API ile kalıcı açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 04da74f23663313e70e54fd4f2f9e5f005791cff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="working-with-groups-in-signalr-1x"></a>SignalR grupları ile çalışma 1.x
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)

> Bu konu, kullanıcıları gruplara ekleyin ve grup üyeliği bilgilerini kalıcı açıklar.


## <a name="overview"></a>Genel Bakış

SignalR gruplarında yayın iletileri belirtilen kümelerine bağlı istemciler için bir yöntem sağlar. Bir grup herhangi bir sayıda istemcileri içerebilir ve bir istemci grupları herhangi bir sayıda üyesi olabilir. Açıkça grupları oluşturmanız gerekmez. Etkin bir grup Groups.Add çağrıda adını belirtin ilk kez otomatik olarak oluşturulur ve bu üyelik son bağlantı kaldırdığınızda silinir. Grupları kullanma için giriş için bkz [Hub sınıfından grup üyeliğini yönetme](index.md) hub API'sindeki - Server Kılavuzu.

Bir grup üyeliği listesinin veya gruplarının bir listesini almak için hiçbir API yoktur. SignalR ileti istemciler ve pub/alt modelini temel alan gruplar gönderir ve sunucu grupları ve grup üyeliklerine listelerini içermez. Bir web grubu için bir düğüm eklediğinizde, yeni bir düğüme yayılması SignalR tutar herhangi bir durum olduğundan bu ölçeklenebilirlik, en üst düzeye çıkarmanıza yardımcı olur.

Kullanarak bir grup kullanıcı eklediğinizde `Groups.Add` yöntemi, kullanıcının geçerli bağlantı süresince bu gruba yönelik iletileri alır, ancak bu gruba kullanıcının üyelik geçerli bağlantıyı kalıcı yapılmaz. Gruplar ve grup üyeliği bilgilerini kalıcı olarak korumak istiyorsanız, bir veritabanı veya Azure tablo depolama alanı gibi bir havuzda verileri depolamanız gerekir. Ardından, bir kullanıcının uygulamanıza her bağlandığında, depodan kullanıcının ait olduğu grupları almak ve el ile kullanıcıyı bu gruplara ekleyin.

Geçici bozulma sonra yeniden bağlanmayı, kullanıcı daha önce atanan grupları otomatik olarak yeniden'e birleştirir. Otomatik olarak bir grup katılmanız yalnızca yeniden bağlanma, yeni bir bağlantı kurmadan olduğunda geçerlidir. Dijital olarak imzalanmış bir belirteç daha önce atanan grupları listesini içeren istemciden geçirilir. Kullanıcı istenen grubuna ait olup olmadığını doğrulamak istiyorsanız, varsayılan davranışı geçersiz kılabilirsiniz.

Bu konu aşağıdaki bölümleri içermektedir:

- [Ekleme ve kullanıcıları kaldırma](#add)
- [Bir grubun üyeleri çağırma](#call)
- [Grup üyeliği bir veritabanında saklamak](#storedatabase)
- [Azure tablo depolama içinde grup üyeliği](#storeazuretable)
- [Grup üyeliği bağlanırken Yönlendirilme doğrulanıyor](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Ekleme ve kullanıcıları kaldırma

Eklemek veya kullanıcıların bir gruptan kaldırmak için arama [Ekle](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) veya [kaldırmak](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) yöntemleri ve kullanıcının bağlantı kimliği ve grubun adını parametreler olarak geçirin. Bağlantı sona erdiğinde bir kullanıcıyı gruptan el ile kaldırmak zorunda değildir.

Aşağıdaki örnekte gösterildiği `Groups.Add` ve `Groups.Remove` Hub yöntemlerinde kullanılan yöntemleri.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add` Ve `Groups.Remove` zaman uyumsuz bir yöntem yürütülemez.

Bir istemci bir gruba eklemek ve hemen Grup seçeneğini kullanarak bir ileti istemciye göndermek istiyorsanız, Groups.Add yöntemi ilk tamamladığından emin yapmanız gerekir. Aşağıdaki kod örnekleri, .NET 4.5 ve .NET 4'te çalışan kod kullanarak bir tane çalışan kod kullanarak bir tane bunun nasıl yapılacağını gösterir.

#### <a name="asynchronous-net-45-example"></a>Zaman uyumsuz .NET 4.5 örneği

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Zaman uyumsuz .NET 4 örnek

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

Genel olarak, dahil `await` çağrılırken `Groups.Remove` yöntemi kaldırmaya çalıştığınız bağlantı kimliği artık kullanılabilir olabileceği için. Bu durumda, `TaskCanceledException` istek zaman aşımına sonra atılır. Ekleyebileceğiniz uygulamanızı gruba bir iletiyi göndermeden önce kullanıcı grubundan kaldırıldı emin olmalısınız varsa, `await` Groups.Remove ve ardından catch önce `TaskCanceledException` durum özel durumu.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Bir grubun üyeleri çağırma

İleti tüm bir grubu üyeleri veya yalnızca belirtilen grubunun üyeleri aşağıdaki örneklerde gösterildiği gibi gönderebilirsiniz.

- **Tüm** bağlı istemciler belirtilen grubunda. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Tüm bağlı istemcileri belirtilen gruptaki **dışındaki belirtilen istemcilerin**, bağlantı kimliği tarafından tanımlanan 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Tüm bağlı istemcileri belirtilen gruptaki **çağıran istemci dışındaki**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Grup üyeliği bir veritabanında saklamak

Aşağıdaki örnekler bir veritabanında grup ve kullanıcı bilgilerini korumak nasıl gösterir. Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte Entity Framework kullanarak modelleri tanımlamak nasıl gösterir. Bu varlık modelleri veritabanı tabloları ve alanları karşılık gelir. Veri yapısını uygulamanızın gereksinimlerine bağlı olarak önemli ölçüde farklılık. Bu örnek adlı bir sınıf içerir `ConversationRoom` spor veya bahçesi oluşturma gibi farklı konularla ilgili görüşmeleri katılmak kullanıcıların sağlayan bir uygulama için benzersiz olacak. Bu örnek ayrıca bağlantılar için bir sınıf içerir. Bağlantı sınıfı grup üyeliği izlemek için kesinlikle gerekli değildir ancak sık kullanıcılar izleme için güçlü bir çözüm bir parçasıdır.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Sonra hub'ı, Grup ve kullanıcı bilgilerini veritabanından ve el ile kullanıcı uygun gruplara ekleyin. Örneğin, kullanıcı bağlantılarını izlemek için kod içermez. Bu örnekte, `await` önce anahtar sözcüğü uygulanmaz `Groups.Add` grubunun üyeleri için bir ileti hemen gönderilmez olduğundan. Yeni üye hemen ekledikten sonra grubunun tüm üyeleri için bir ileti göndermek istiyorsanız, uygulamak istediğiniz `await` zaman uyumsuz işlemi tamamlandı emin olmak için anahtar sözcüğü.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Azure tablo depolama içinde grup üyeliği

Grup ve kullanıcı bilgilerini depolamak için Azure tablo depolaması kullanarak bir veritabanını kullanmaya benzer. Aşağıdaki örnekte kullanıcı adı ve grup adı depolayan Tablo varlığı gösterir.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

Hub'ı, kullanıcı bağlandığında, atanan grupları alır.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Grup üyeliği bağlanırken Yönlendirilme doğrulanıyor

Varsayılan olarak, SignalR otomatik olarak bir kullanıcı uygun gruplara durumundan zaman bağlantı bırakılan ve bağlantı zaman aşımına uğramadan önce yeniden kurdu gibi geçici, yeniden bağlanmayı yeniden atar. Kullanıcı grubu bilgilerini bir belirteç içine bağlanırken Yönlendirilme geçirilir ve belirtecini sunucuda doğrulanır. Kullanıcı gruplarına yeniden katılma için doğrulama işlemi hakkında daha fazla bilgi için bkz: [bağlanırken Yönlendirilme gruplarına yeniden katılma](index.md).

Genel olarak, otomatik olarak gruplarında yeniden katılmanız varsayılan davranışını kullanmanız gerekir. SignalR grupları hassas verilere erişimi kısıtlamak için bir güvenlik mekanizması olarak tasarlanmamıştır. Ancak, uygulamanızın bir kullanıcının grup üyeliği bağlanırken Yönlendirilme denetleyin, varsayılan davranışı geçersiz kılabilirsiniz. Her yeniden bağlanmak için yerine yalnızca kullanıcı bağlandığında, bir kullanıcının grup üyeliği alınmalıdır çünkü varsayılan davranışı değiştirme veritabanınız için bir yük ekleyebilirsiniz.

Grup üyeliği doğrulamanız gerekir yeniden bağlanma, atanan grupları listesini döndürür yeni bir hub ardışık düzen modül aşağıda gösterildiği gibi oluşturun.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Daha sonra bu modül hub ardışık düzendeki, aşağıda vurgulandığı gibi ekleyin.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
