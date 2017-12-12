---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: "ASP.NET SignalR hub'ları API Kılavuzu - sunucu (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Bu belge için SignalR sürüm 1.1, kod örnekleri demonstratin ile ASP.NET SignalR hub'ları API sunucu tarafı programlama için bir giriş sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: e594dd1ea4ae027cf0b82574fc5a3eb061b1f2e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a>ASP.NET SignalR hub'ları API Kılavuzu - sunucu (SignalR 1.x)
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)

> Bu belge, ASP.NET SignalR hub'ları API sunucu tarafı sürüm 1.1, SignalR için ortak seçeneklerini gösteren kod örnekleri ile programlama giriş bilgileri sağlar.
> 
> SignalR hub'ları API bir sunucuya bağlanan istemciler ve sunucu istemcilerine uzaktan yordam çağrılarını (RPC) yapmanızı sağlar. Sunucu kodu, istemciler tarafından çağrılabilir yöntemlerini tanımlama ve istemci üzerinde çalışan yöntemlerini çağırın. İstemci kodu sunucudan çağrılabilir yöntemlerini tanımlama ve sunucu üzerinde çalışan yöntemlerini çağırın. SignalR, istemci-sunucu tesisat tümünün ilgilenir.
> 
> SignalR kalıcı bağlantılar olarak adlandırılan bir alt düzey API de sunar. Giriş SignalR, hub'lar ve kalıcı bağlantılar için ya da tam bir SignalR uygulamasının nasıl oluşturulacağını gösteren bir öğretici için bkz: [Başlarken SignalR -](index.md).


## <a name="overview"></a>Genel Bakış

Bu belgede aşağıdaki bölümler yer alır:

- [SignalR rota kaydetmek ve SignalR seçeneklerini yapılandırma](#route)

    - [/Signalr URL'si](#signalrurl)
    - [SignalR seçeneklerini yapılandırma](#options)
- [Oluşturma ve Hub sınıfları kullanma](#hubclass)

    - [Hub nesne ömrü](#transience)
    - [Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin Hub adları](#hubnames)
    - [Birden çok hub'ları](#multiplehubs)
- [İstemcileri çağırabilirsiniz Hub sınıfında yöntemleri tanımlama](#hubmethods)

    - [Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin adlarında yöntemi](#methodnames)
    - [Zaman uyumsuz olarak yürütülecek ne zaman](#asyncmethods)
    - [Aşırı tanımlama](#overloads)
- [İstemci Hub sınıfından yöntemleri çağırmak nasıl](#callfromhub)

    - [Hangi istemcilerin seçerek RPC alırsınız](#selectingclients)
    - [Yöntem adları için doğrulama olmaz derleme zamanı](#dynamicmethodnames)
    - [Ad eşleştirme büyük küçük harf duyarsız yöntemini](#caseinsensitive)
    - [Zaman uyumsuz yürütme](#asyncclient)
- [Hub sınıfından grup üyeliğini yönetme](#groupsfromhub)

    - [Add ve Remove yöntemlerini, zaman uyumsuz yürütme](#asyncgroupmethods)
    - [Grup üyeliği kalıcılığı](#grouppersistence)
    - [Tek kullanıcı grupları](#singleusergroups)
- [Bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını](#connectionlifetime)

    - [OnConnected, OnDisconnected ve OnReconnected olduğunda çağrılır](#onreconnected)
    - [Değil doldurulmuş arayan durumu](#nocallerstate)
- [Bağlam özelliğinden istemcisi hakkında bilgi alma](#contextproperty)
- [Durum istemcileri ve Hub sınıfına arasında geçirmek nasıl](#passstate)
- [Hub sınıfında hataların nasıl işleneceğini](#handleErrors)
- [İstemci yöntemlerini çağırın ve gruplardan Hub sınıfın dışından yönetme](#callfromoutsidehub)

    - [İstemci yöntemleri çağırma](#callingclientsoutsidehub)
    - [Grup üyeliğini yönetme](#managinggroupsoutsidehub)
- [İzlemeyi etkinleştirme](#tracing)
- [Hub ardışık düzen özelleştirme](#hubpipeline)

Program istemcilere nasıl belgeler için aşağıdaki kaynaklara bakın:

- [SignalR hub'ları API Kılavuzu - JavaScript istemci](index.md)
- [SignalR hub'ları API Kılavuzu - .NET istemcisi](index.md)

API başvuru konuları API'si .NET 4.5 sürümüne bağlantılardır. .NET 4 kullanıyorsanız, bkz: [API konuları .NET 4 sürümü](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a>SignalR rota kaydetmek ve SignalR seçeneklerini yapılandırma

İstemcilerin Hub'ınıza bağlanmak için kullanacağı rota tanımlamak için arama [MapHubs](https://msdn.microsoft.com/en-us/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) uygulama başladığında yöntemi. `MapHubs`olan bir [genişletme yöntemi](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) için `System.Web.Routing.RouteCollection` sınıfı. Aşağıdaki örnek SignalR hub'ları rotadaki tanımlamak nasıl gösterir *Global.asax* dosya.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

Bir ASP.NET MVC uygulaması için SignalR işlevselliği ekliyorsanız, SignalR rota diğer rotaların önce eklendiğinden emin olun. Daha fazla bilgi için bkz: [Öğreticisi: SignalR ve MVC 4 ile çalışmaya başlama](index.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>/Signalr URL'si

Varsayılan olarak, istemciler Hub'ınıza bağlanmak için kullanacağı rota URL'dir "/ signalr". (Bu URL için otomatik olarak oluşturulan JavaScript dosyası "/ signalr/hub" URL ile karıştırmayın. Oluşturulan proxy hakkında daha fazla bilgi için bkz: [SignalR hub'ları API Kılavuzu - JavaScript istemci - oluşturulan proxy ve onu sizin için ne yaptığını](index.md).)

Bu temel URL SignalR için kullanılamaz hale olağanüstü durumlar olabilir; Örneğin, bir klasör adında projenizde var. *signalr* ve adını değiştirmek istemiyorsanız. Bu durumda, aşağıdaki örneklerde gösterildiği gibi temel URL değiştirebilirsiniz (Değiştir "/ signalr" örnek kodda, istenen URL ile).

**URL'yi belirtir sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

**URL (ile oluşturulan proxy) belirtir JavaScript istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

**(Olmadan oluşturulan proxy) URL'yi belirtir JavaScript istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

**URL'yi belirtir .NET istemci kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>SignalR seçeneklerini yapılandırma

Overloads `MapHubs` yöntemini etkinleştirmek, özel bir URL, özel bağımlılık çözümleyici ve aşağıdaki seçenekleri belirtin:

- Etki alanları arası çağrılar tarayıcı istemcilerinden etkinleştirin.

    Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantısı `http://contoso.com/signalr`. Varsa sayfasından `http://contoso.com` bir bağlantı kurar `http://fabrikam.com/signalr`, yani etki alanları arası bağlantı. Güvenlik nedenleriyle, etki alanları arası bağlantılar, varsayılan olarak devre dışıdır. Daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - JavaScript istemci - etki alanları arası bağlantı kurmak nasıl](index.md).
- Ayrıntılı hata iletilerini etkinleştir.

    Hatalar oluştuğunda, SignalR varsayılan davranışını ne hakkında ayrıntılar olmadan bir bildirim iletisi istemcilere göndermektir. Kötü niyetli kullanıcılar, uygulamanızın saldırıları bilgileri kullanmak olabilir çünkü istemciler için ayrıntılı hata bilgileri gönderme üretimde önerilmez. Sorun giderme için geçici olarak daha bilgilendirici hata raporlamayı etkinleştirmek için bu seçeneği kullanabilirsiniz.
- Otomatik olarak oluşturulan JavaScript proxy dosyaları devre dışı bırakın.

    Varsayılan olarak, yanıt URL "/ signalr/hubs" olarak Hub sınıfları için proxy ile bir JavaScript dosyası oluşturulur. JavaScript proxy'leri kullanmak istemiyorsanız veya bu dosyayı el ile oluşturmak ve fiziksel bir dosyaya istemcileriniz başvurmak istiyorsanız proxy oluşturması devre dışı bırakmak için bu seçeneği kullanabilirsiniz. Daha fazla bilgi için bkz: [SignalR hub'ları API Kılavuzu - JavaScript istemci - için SignalR fiziksel bir dosya oluşturmak nasıl oluşturulan proxy](index.md).

Aşağıdaki örnek, bir çağrıda SignalR bağlantı URL'si ve bu seçeneklerini belirtmek gösterilmektedir `MapHubs` yöntemi. Özel bir URL belirtmek için Değiştir "/ signalr" kullanmak istediğiniz URL ile örnekte.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Oluşturma ve Hub sınıfları kullanma

Bir Hub oluşturmak için türeyen bir sınıf oluşturun [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). Aşağıdaki örnek, sohbet uygulaması için basit bir Hub sınıfı gösterir.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

Bu örnekte, bir bağlı istemci çağırabilirsiniz `NewContosoChatMessage` yöntemi ve yaptığında, alınan verileri bağlanan tüm istemciler için yayımladınız.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Hub nesne ömrü

Hub sınıfının örneği yok ya da kendi kodunuzu sunucuda yöntemlerinden çağırmanıza; Tüm sizin için SignalR hub'ları ardışık düzen tarafından yapılır. SignalR ne zaman bir istemci bağlanır, bağlantısını keser veya sunucuya bir yöntem çağrısı yapar gibi bir Hub işlemi işlemek için gereken her zaman, Hub sınıfının yeni bir örneğini oluşturur.

Hub sınıfının örnekleri geçici olduğundan, bir yöntem çağrısı sonraki durumunu korumak için kullanamazsınız. Her zaman sunucu yöntemi çağrısı ileti bir istemciden Hub sınıfı işlemlerinizi yeni bir örneğini alır. Birden çok bağlantıları ve yöntem çağrıları aracılığıyla durumunu korumak için Hub sınıfına veya türünden türemez farklı bir sınıf bir veritabanı veya statik değişkeni gibi bazı başka bir yöntem kullanın `Hub`. Bellek verileri devam ederse, uygulama etki alanı geri dönüştürüldüğünde Hub sınıfı üzerinde statik bir değişken gibi bir yöntem kullanarak verileri kaybolur.

Hub sınıf dışında çalışan kendi kodundan istemcilere iletileri göndermek istiyorsanız, bir Hub sınıfı örneğini oluşturarak bunu yapamazsınız, ancak Hub sınıfınız için SignalR bağlamı nesneye bir başvurusu alarak yapabilirsiniz. Daha fazla bilgi için bkz: [istemci yöntemlerini çağırın ve Hub sınıfına dışında gruplarından yönetmek nasıl](#callfromoutsidehub) bu konuda daha sonra.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin Hub adları

Varsayılan olarak, JavaScript istemcilerinin sınıf adı başlamalıdır sürümünü kullanarak hub'lara bakın. SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun. Önceki örneği olarak adlandırılan `contosoChatHub` JavaScript kodu.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemci**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

İstemcilerin kullanın, eklemek farklı bir ad belirtmek istiyorsanız `HubName` özniteliği. Kullandığınızda, bir `HubName` özniteliği, JavaScript istemcilerde ortası büyük için ad değişiklik yoktur.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemci**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Birden çok hub'ları

Bir uygulamada birden çok Hub sınıfları tanımlayabilirsiniz. Bunu yaptığınızda, bağlantı paylaşılan ancak grupları ayrı şunlardır:

- Tüm istemciler aynı URL'ye hizmetinizle bir SignalR bağlantısı kurmak için kullanır ("/ signalr" veya bir belirtilmişse özel URL'nizi), hizmet tarafından tanımlanan ve bağlantı tüm hub'ları için kullanılır.

    Birden çok hub'ları tüm Hub işlevlerini tek bir sınıf tanımlama için karşılaştırılması için herhangi bir performans farkı yoktur.
- Tüm hub'ları aynı HTTP isteği bilgi alın.

    Tüm hub'ı aynı bağlantıyı paylaştığında olduğundan, sunucunun alır yalnızca HTTP istek bilgileri ne SignalR bağlantı kurar özgün HTTP isteği gelen kalır. Bilgi bir sorgu dizesi belirterek istemciden sunucuya geçirmek için bağlantı isteğini kullanırsanız, farklı sorgu dizeleri için farklı hub sağlayamaz. Tüm hub'ları aynı bilgileri alır.
- Bir dosyadaki tüm hub'lara yönelik proxy'leri oluşturulan JavaScript proxy'leri dosyasını içerir.

    JavaScript proxy'leri hakkında daha fazla bilgi için bkz: [SignalR hub'ları API Kılavuzu - JavaScript istemci - oluşturulan proxy ve onu sizin için ne yaptığını](index.md).
- Grupları hub içinde tanımlanmıştır.

    Tanımlayabileceğiniz SignalR öğesinde bağlı istemciler alt kümeleri için yayın için Grup adı. Grupları, her Hub için ayrı ayrı tutulur. Örneğin, "Yöneticiler" adlı bir grup istemciler için bir kümesini içerir, `ContosoChatHub` sınıfı ve aynı grubu adını istemciler için farklı bir dizi bakın, `StockTickerHub` sınıfı.

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>İstemcileri çağırabilirsiniz Hub sınıfında yöntemleri tanımlama

İstemciden aranabilir olmasını istediğiniz hub'ındaki bir yöntem kullanıma sunmak için aşağıdaki örneklerde gösterildiği gibi genel bir yöntem bildirin.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Dönüş türü ve tüm C# yönteminde olduğu gibi karmaşık türler ve diziler dahil olmak üzere parametreleri de belirtebilirsiniz. Alırsınız parametrelerde veya çağırana döndüren herhangi bir veri istemci ve sunucu arasında JSON kullanarak bildirilir ve karmaşık nesne bağlama ve nesne dizileri SignalR otomatik olarak yönetir.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin adlarında yöntemi

Varsayılan olarak, JavaScript istemcilerinin yöntem adı başlamalıdır sürümünü kullanarak Hub yöntemlerine bakın. SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemci**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

İstemcilerin kullanın, eklemek farklı bir ad belirtmek istiyorsanız `HubMethodName` özniteliği.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemci**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Zaman uyumsuz olarak yürütülecek ne zaman

Yöntemi uzun süre çalışan olması veya çalışmak olup olmadığını, veritabanı arama veya bir web hizmeti çağrısı gibi bekleme içeren, döndürerek Hub yöntemini zaman uyumsuz hale bir [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (yerine `void` dönüş) veya [ Görev&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/dd321424.aspx) nesne (yerine `T` dönüş türü). Döndüğünüzde bir `Task` SignalR yöntemi nesnesinden bekler `Task` tamamlamak için ve bu yüzden yöntem çağrısı istemci kodu nasıl içinde herhangi bir fark ardından sarmalanmamış sonuç istemciye geri gönderir.

Bir Hub yöntemini olmasını zaman uyumsuz WebSocket taşıma kullandığında bağlantıyı engelliyor önler. Hub yönteminin tamamlayana kadar bir Hub yöntemini zaman uyumlu olarak yürütür ve WebSocket taşıma olduğunda, aynı istemciden hub yöntemlerine yönelik sonraki çağrılarını engellenir.

Aynı yöntem eşzamanlı çalışacak biçimde kodlanmış veya zaman uyumsuz olarak, her iki sürümü çağırmak için çalışır JavaScript istemci kodu ve ardından aşağıdaki örnekte gösterilmiştir.

**Zaman uyumlu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

**Zaman uyumsuz - ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Oluşturulan proxy kullanarak JavaScript istemci**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

ASP.NET 4.5 içinde zaman uyumsuz yöntemleri kullanma hakkında daha fazla bilgi için bkz: [kullanarak ASP.NET MVC 4'te zaman uyumsuz yöntemleri](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Aşırı tanımlama

Bir yöntemi için aşırı tanımlamak istiyorsanız, her aşırı parametre sayısı farklı olmalıdır. Farklı parametre türleri belirterek bir aşırı ayırt Hub sınıfınız derlenir ancak SignalR hizmet çağrısı aşırı birini istemcileri çalıştığınızda çalışma zamanında bir özel durum oluşturur.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>İstemci Hub sınıfından yöntemleri çağırmak nasıl

İstemci sunucudan yöntemleri çağırmak için kullanın `Clients` Hub sınıfınız yönteminde bir özellik. Aşağıdaki örnek, çağıran sunucu kodu gösterir `addNewMessageToPage` tüm bağlı istemcileri ve bir JavaScript istemci yöntemi tanımlar istemci kodu.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

**Oluşturulan proxy kullanarak JavaScript istemci**

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

Bir istemci yöntemden dönüş değeri alınamıyor; sözdizimi gibi `int x = Clients.All.add(1,1)` çalışmıyor.

Karmaşık türler ve diziler parametreleri için belirtebilirsiniz. Aşağıdaki örnek karmaşık bir tür bir yöntem parametresi istemcisinde geçirir.

**Karmaşık bir nesne kullanarak bir istemci yöntemini çağıran sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

**Karmaşık nesne tanımlar sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemci**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Hangi istemcilerin seçerek RPC alırsınız

İstemcileri özelliği döndürür bir [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) hangi istemcilerin RPC alacak belirtmek için çeşitli seçenekler sağlayan nesne:

- Bağlanan tüm istemciler.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- Yalnızca çağıran istemci.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- Çağıran istemci dışındaki tüm istemcilerin.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- Bağlantı kimliği ile tanımlanan belirli bir istemci

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    Bu örnek çağırır `addContosoChatMessageToPage` çağıran istemci hakkında ve kullanarak aynı etkiye sahip `Clients.Caller`.
- Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- Belirli bir grubun tüm bağlı istemcileri.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- Bağlantı kimliği ile tanımlanan belirtilen istemciler dışında belirtilen gruptaki tüm bağlı istemcileri

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- Belirtilen bir grubundaki tüm bağlı istemcileri çağıran istemci dışındaki.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Yöntem adları için doğrulama olmaz derleme zamanı

Yöntem adı IntelliSense veya derleme zamanı doğrulamasını yoktur anlamına gelir dinamik bir nesne olarak yorumlanır. İfade, çalışma zamanında değerlendirilir. Yöntem çağrısının yürütüldüğünde, kendisine SignalR yöntem adı ve parametre değerlerini istemciye gönderir ve istemci bir yöntemi varsa adı ile eşleşen yöntem çağrılır ve parametre değerlerini geçirildi. Eşleşen bir yöntem istemcide bulunursa, herhangi bir hata oluştu. Bir istemci yöntemini çağırdığınızda, arka planda istemcisi için SignalR ileten veri biçimi hakkında daha fazla bilgi için bkz: [SignalR giriş](index.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Ad eşleştirme büyük küçük harf duyarsız yöntemini

Yöntemi ad eşleştirme büyük küçük harfe duyarlıdır. Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütecek `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, veya `addContosoChatMessageToPage` istemci üzerinde.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Zaman uyumsuz yürütme

Çağrı yöntemini zaman uyumsuz olarak yürütür. Bir istemci için bir yöntem çağrısı, belirtmediğiniz sürece istemciler veri aktarırken kod sonraki satırların tamamlamak SignalR için beklenmeden hemen yürütülmez sonra gelen herhangi bir kod yöntemi tamamlanmasını beklemeniz gerekir. İki istemci yöntemleri sırayla yürütmek nasıl Göster aşağıdaki kod örnekleri, kullanarak .NET 4.5, çalışır kod, diğeri kullanarak .NET 4'te bu works kod.

**.NET 4.5 örneği**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

**.NET 4 örnek**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

Kullanırsanız `await` veya `ContinueWith` kodun sonraki satırında, yürütülmeden önce bir istemci yöntemi sonlanana kadar beklemeniz için gelmez kodun sonraki satırında, yürütülmeden önce istemcileri gerçekte iletiyi alır. Bir istemci yöntem çağrısının "tamamlama" yalnızca SignalR ileti göndermek için gereken her şeyi yaptığına anlamına gelir. İstemcileri iletisini aldığınızı doğrulama gerekiyorsa, bu mekanizma kendiniz program sahip. Örneğin, aşağıdaki kodu yazabilirsiniz bir `MessageReceived` yöntemi hub'ı hem de `addContosoChatMessageToPage` çağrı istemcide yöntemi `MessageReceived` , yaptıktan sonra iş yapmanız gereken istemcide. İçinde `MessageReceived` hub'ı gerçek istemci alımı ve özgün yöntem çağrısı işlenmesini hangi iş bağlıdır yapabilirsiniz.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Yöntem adı bir dize değişkeni kullanma

Cast yöntemi adı olarak bir dize değişkeni kullanarak bir istemci yöntemi çağırma istiyorsanız `Clients.All` (veya `Clients.Others`, `Clients.Caller`, vs.) için `IClientProxy` ve ardından arama [Invoke (methodName, bağımsız değişken...) ](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Hub sınıfından grup üyeliğini yönetme

SignalR gruplarında yayın iletileri belirtilen kümelerine bağlı istemciler için bir yöntem sağlar. Bir grup herhangi bir sayıda istemcileri içerebilir ve bir istemci grupları herhangi bir sayıda üyesi olabilir.

Grup üyeliğini yönetmek için [Ekle](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ve [kaldırmak](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) tarafından sağlanan yöntemleri `Groups` Hub sınıfın özelliği. Aşağıdaki örnekte gösterildiği `Groups.Add` ve `Groups.Remove` istemci kodu tarafından çağrılan Hub yöntemlerini kullanılan yöntemleri ve ardından onları çağıran tarafından JavaScript istemci kodu.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

**Oluşturulan proxy kullanarak JavaScript istemci**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

Açıkça grupları oluşturmanız gerekmez. Etkin bir grup çağrıda adını belirttiğiniz ilk kez otomatik olarak oluşturulur `Groups.Add`, ve bu üyelik son bağlantı kaldırdığınızda silinir.

Bir grup üyeliği listesinin veya gruplarının bir listesini almak için hiçbir API yoktur. SignalR istemcileri ve gruplara göre iletileri gönderen bir [pub/alt model](http://en.wikipedia.org/wiki/Publish/subscribe), ve sunucu grupları ve grup üyeliklerine listelerini içermez. Bir web grubu için bir düğüm eklediğinizde, yeni bir düğüme yayılması SignalR tutar herhangi bir durum olduğundan bu ölçeklenebilirlik, en üst düzeye çıkarmanıza yardımcı olur.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Add ve Remove yöntemlerini, zaman uyumsuz yürütme

`Groups.Add` Ve `Groups.Remove` zaman uyumsuz bir yöntem yürütülemez. Bir istemci bir gruba eklemek ve hemen Grup seçeneğini kullanarak bir ileti istemciye göndermek istediğinizden emin olmak varsa `Groups.Add` yöntemi bitmeden önce. Aşağıdaki kod örnekleri, .NET 4.5 ve .NET 4'te çalışan kod kullanarak bir tane çalışan kod kullanarak bir tane nasıl Göster

**.NET 4.5 örneği**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

**.NET 4 örnek**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Grup üyeliği kalıcılığı

SignalR bağlantıları izler, kullanıcıları değil, dolayısıyla bir kullanıcının kullanıcı her bağlandığında, bir bağlantı aynı grupta olmasını istediğiniz, çağrı zorunda `Groups.Add` edildiğinde kullanıcı yeni bir bağlantı kurar.

Geçici bir bağlantı kaybı sonra bazen SignalR bağlantısı otomatik olarak geri yükleyebilirsiniz. Bu durumda, yeni bir bağlantı kurmadan aynı bağlantı SignalR geri yüklüyor ve bu nedenle istemcinin Grup üyeliğini otomatik olarak geri. Bağlantı durumu grup üyelikleri de dahil olmak üzere her istemci için istemcinin gidiş-dönüş olduğundan bu geçici sonu nedeni sunucu yeniden veya hata olduğunda bile mümkündür. Bir sunucu bağlantısı kesilse ve bağlantı zaman aşımına uğramadan önce yeni bir sunucu tarafından değiştirilirse, istemci otomatik olarak yeni sunucuya yeniden bağlanma ve bir üyesidir gruplarında yeniden kaydolun.

Bağlantı otomatik olarak bağlantı kaybı sonra geri yüklenemiyor veya bağlantı zaman aşımına uğradığında veya (örneğin, bir tarayıcı için yeni bir sayfa gittiğinde) istemci kestiğinde, grup üyelikleri kaybolur. Kullanıcı bir sonraki bağlanışında yeni bir bağlantı olur. Aynı kullanıcı yeni bir bağlantı kurduğunda grup üyeliklerini korumasına kullanıcılar ve gruplar ilişkilendirmeleri izlemek ve grup üyeliklerini bir kullanıcı yeni bir bağlantı kurar her zaman geri yüklemek, uygulamanız gerekir.

Bağlantıları ve tutarsızlıklara hakkında daha fazla bilgi için bkz: [bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını](#connectionlifetime) bu konuda daha sonra.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Tek kullanıcı grupları

Hangi kullanıcı bir ileti gönderdi ve hangi kullanıcıları bir ileti almalıdır bilmek için kullanıcıları ve bağlantıları arasındaki ilişkilendirmeleri izlemek SignalR genellikle kullanan uygulamalar vardır. Gruplar iki yaygın olarak kullanılan desenleri birini yapmak için kullanılır.

- Tek kullanıcı grupları.

    Grup adı olarak kullanıcı adı belirtin ve kullanıcı bağlandığında veya yeniden bağlandığında her zaman geçerli bağlantı kimliği grubuna ekleyin. Kullanıcıya iletileri göndermek için Grup gönderin. Bu yöntem bir dezavantajı, grup kullanıcının çevrimiçi veya çevrimdışı olup olmadığını öğrenmek için bir yol sağlamaz ' dir.
- Kullanıcı adı ve bağlantı kimlikleri ilişkilendirmeleri izler.

    Her bir kullanıcı adı ve bir veya daha fazla bağlantı kimlikleri arasında bir ilişki bir sözlük veya veritabanında depolamak ve kullanıcı bağlandığında veya bağlantısı kesildiğinde her zaman depolanan verileri güncelleştirin. Kullanıcıya iletileri göndermek için Bağlantı kimliklerinin belirtin. Bu yöntem bir dezavantajı, daha fazla bellek sürdüğünü ' dir.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını

Bir kullanıcı veya bağlı olup olmadığını izler ve kullanıcı adı ve bağlantı kimlikleri arasındaki ilişkiyi izlemek için bağlantı ömür olayları işlemek için tipik nedenleri açıklanmaktadır. İstemcileri bağlanın veya bağlantıyı kesin zaman kendi kodunuzu çalıştırmak için geçersiz kılma `OnConnected`, `OnDisconnected`, ve `OnReconnected` Hub'ın sanal yöntemler sınıfı, aşağıdaki örnekte gösterildiği gibi.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>OnConnected, OnDisconnected ve OnReconnected olduğunda çağrılır

Bir tarayıcı yeni bir sayfaya gider her zaman yeni bir bağlantı kurulması SignalR yürütülecek yani sahip `OnDisconnected` yöntemi arkasından `OnConnected` yöntemi. Yeni bir bağlantı kurulduğunda SignalR her zaman yeni bir bağlantı kimliği oluşturur.

`OnReconnected` Yaşandığında geçici sonu SignalR otomatik olarak, ne zaman bir kablo geçici olarak bağlantısı kesilir ve bağlantısı zaman aşımına uğramadan önce bağlantısı yeniden kurulmuş gibi kurtarabilirsiniz bağlantılar yöntemi çağrılır. `OnDisconnected` Yöntemi, istemci bağlantısı ve SignalR olamaz otomatik olarak yeniden, bir tarayıcı için yeni bir sayfa zaman gider gibi olduğunda çağrılır. Bu nedenle, belirli bir istemcinin olayları olası dizisidir `OnConnected`, `OnReconnected`, `OnDisconnected`; veya `OnConnected`, `OnDisconnected`. Sıra görmezsiniz `OnConnected`, `OnDisconnected`, `OnReconnected` belirli bir bağlantı için.

`OnDisconnected` Yöntemi olmayan adlı bir sunucu zaman arıza gibi bazı senaryolarda veya uygulama etki alanı geri alır. Başka bir sunucuya gelinceye veya uygulama etki alanı kendi Geri Dönüşüm tamamlandığında, bazı istemciler yeniden bağlanın ve yangın mümkün olabilir `OnReconnected` olay.

Daha fazla bilgi için bkz: [anlama ve SignalR bağlantısı ömrü olaylarını işleme](index.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Değil doldurulmuş arayan durumu

Bağlantı ömrü olay işleyicisi yöntemleri içine herhangi bir durum anlamına sunucusundan denir `state` istemcideki nesne içinde doldurulmayacak `Caller` sunucudaki özelliği. Hakkında bilgi için `state` nesne ve `Caller` özelliği, bkz: [durumu istemcileri ve Hub sınıfına arasında geçirmek nasıl](#passstate) bu konuda daha sonra.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Bağlam özelliğinden istemcisi hakkında bilgi alma

İstemcisi hakkında bilgi almak için `Context` Hub sınıfın özelliği. `Context` Özelliği döndürür bir [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) aşağıdaki bilgilere erişim sağlayan nesnesi:

- Çağıran istemcinin bağlantı kimliği.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    Bağlantı kimliği (kendi kodunuzu değeri belirtemezsiniz) SignalR tarafından atanan bir GUID değeridir. Her bağlantı ve uygulamanızda birden çok hub'lar varsa tüm hub tarafından kullanılan kimliği aynı bağlantı için bir bağlantı kimliği yok.
- HTTP üstbilgisi verileri.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    HTTP üstbilgileri elde edebilirsiniz `Context.Headers`. Aynı şey birden fazla başvuru nedeni `Context.Headers` ilk, oluşturulan `Context.Request` özelliği daha sonra eklenen ve `Context.Headers` geriye dönük uyumluluk için korunur.
- Dize verilerini sorgu.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    Sorgu dizesi verileri elde edebilirsiniz `Context.QueryString`.

    Bu özellik alma sorgu dizesi bir SignalR bağlantısı oluşturulmuş olan HTTP isteği kullanılan adrestir. İstemci hakkındaki verileri istemciden sunucuya geçirmek için kullanışlı bir yoldur bağlantı yapılandırarak, sorgu dizesi parametreleri istemcisinde ekleyebilirsiniz. Aşağıdaki örnek, oluşturulan proxy kullandığınızda bir JavaScript istemci bir sorgu dizesi eklemek için bir yol gösterir.

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    Sorgu dizesi parametreleri ayarlama hakkında daha fazla bilgi için bkz. API kılavuzları için [JavaScript](index.md) ve [.NET](index.md) istemciler.

    Sorgu dizesi verileri SignalR tarafından dahili olarak kullanılan bazı değerler birlikte bağlantı için kullanılan aktarım yöntemi bulabilirsiniz:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    Değeri `transportMethod` "webSockets", "serverSentEvents", "foreverFrame" veya "longPolling" olacaktır. Bu değer işaretlerseniz unutmayın `OnConnected` olay işleyicisi yöntemi, bazı senaryolarda bağlantı için son anlaşılan taşıma yöntemi olmayan bir taşıma değeri başlangıçta alabilirsiniz. Bu durumda yöntemi bir özel durum oluşturur ve daha sonra yeniden son taşıma yöntemi kurulduğunda çağrılır.
- Tanımlama bilgileri.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Tanımlama bilgilerini de alabilirsiniz `Context.RequestCookies`.
- Kullanıcı bilgileri.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- İsteğin HttpContext nesnesi:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    Alma yerine bu yöntemi kullanmak `HttpContext.Current` almak için `HttpContext` SignalR bağlantı için nesnesi.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Durum istemcileri ve Hub sınıfına arasında geçirmek nasıl

İstemci proxy sağlayan bir `state` içinde depolayabilirsiniz sunucusuna her yöntem çağrısı ile iletilmesi istediğiniz veri nesnesi. Sunucuda bu verilerine erişebilir `Clients.Caller` istemciler tarafından çağrılan Hub yöntemlerini özelliği. `Clients.Caller` Özelliği için bağlantı ömrü olay işleyicisi yöntemleri değil doldurulmuş `OnConnected`, `OnDisconnected`, ve `OnReconnected`.

Oluşturma veya güncelleştirme verilerde `state` nesne ve `Clients.Caller` özelliği her iki yönde de çalışır. Sunucu değerlerde güncelleştirebilir ve istemciye geçirilir.

Aşağıdaki örnek durumu iletilmesi için her yöntem çağrısı sunucusuyla depolar JavaScript istemci kodu gösterir.

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

Aşağıdaki örnek, bir .NET istemci eşdeğeri olan kodu gösterir.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

Hub sınıfınızda bu verilerine erişebilir `Clients.Caller` özelliği. Aşağıdaki örnek, önceki örnekte başvurulan durumu alır kodu gösterir.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> Bu mekanizma kalıcı durumu için her şeyi içine itibaren büyük miktarlarda verilerin amaçlanmamıştır `state` veya `Clients.Caller` özelliktir gidiş-dönüş ile her yöntem çağırma. Kullanıcı adlarını veya sayaçları gibi küçük öğeler için kullanışlıdır.


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Hub sınıfında hataların nasıl işleneceğini

Hub sınıfı yöntemlerinizi oluşan hataları işlemek için aşağıdakilerden birini veya her ikisi de aşağıdaki yöntemlerden birini kullanın:

- Try-catch bloklarını yöntemi kodunuzu kaydırma ve özel durum nesnesi oturum açın. Hata ayıklama amacıyla istemciye özel gönderebilir, ancak güvenlik için üretim istemciler için ayrıntılı bilgi gönderme nedeniyle önerilmez.
- İşleme bir hub ardışık düzen modül oluşturma [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) yöntemi. Aşağıdaki örnek hub ardışık düzenine Modülü yerleştirir Global.asax kodda ve ardından hatalarını günlüğe bir ardışık düzen modülü gösterir.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

Hub ardışık düzen modüllerine hakkında daha fazla bilgi için bkz: [hub ardışık düzen özelleştirmek nasıl](#hubpipeline) bu konuda daha sonra.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>İzlemeyi etkinleştirme

System.diagnostics öğesi sunucu-tarafı izlemeyi etkinleştirmek için Web.config dosyanıza Bu örnekte gösterildiği gibi ekleyin:

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

Visual Studio'da uygulamayı çalıştırdığınızda, günlükleri görüntüleyebilirsiniz **çıkış** penceresi.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>İstemci yöntemlerini çağırın ve gruplardan Hub sınıfın dışından yönetme

İstemci Hub sınıfınızın daha farklı bir sınıftan yöntemleri çağırmak için Hub için SignalR bağlamı nesneye bir başvurusu alın ve, istemcide yöntemlerini çağıran veya grupları yönetmek için kullanın.

Aşağıdaki örnek `StockTicker` sınıfı context nesnesi alır, sınıfının bir örneğini depolar, bir statik özellik sınıf örneği depolar ve çağırmak için singleton sınıfı örneği bağlamından kullanır `updateStockPrice` istemciler üzerinde yöntemi adlı bir Hub'ına bağlı `StockTickerHub`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

Uzun süreli bir nesne bağlamı birden çok kez kullanmanız gerekiyorsa, bir kez başvurusu alın ve yerine her zaman tekrar alma kaydedin. Bağlamı bir kez alma SignalR içinde ve Hub yöntemlerine istemci yöntem çağrılarına olun aynı sırayla istemcilere iletileri gönderir sağlar. SignalR bağlamı için bir hub'ı kullanmak nasıl oluşturulduğunu gösteren bir öğretici için bkz [Server yayını ASP.NET SignalR ile](index.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>İstemci yöntemleri çağırma

Hangi istemcilerin RPC alacak belirtebilirsiniz, ancak bir Hub sınıftan çağırdığınızda değerinden daha az seçeneğe sahiptir. Bunun nedeni herhangi bir yöntem gibi geçerli bağlantı kimliği bilgisi gerektiren bağlamı bir istemciden belirli çağrısıyla ilişkili olmadığından emin olup `Clients.Others`, veya `Clients.Caller`, veya `Clients.OthersInGroup`, kullanılabilir değil. Aşağıdaki seçenekler mevcuttur:

- Bağlanan tüm istemciler.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- Bağlantı kimliği ile tanımlanan belirli bir istemci

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- Belirli bir grubun tüm bağlı istemcileri.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- Bağlantı kimliği ile tanımlanan belirtilen istemciler dışında belirtilen gruptaki tüm bağlı istemcileri

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

Hub sınıfınızda yöntemleri Hub bileşen sınıfına arıyorsanız, geçerli bağlantı kimliği geçirmek ve ile kullanan `Clients.Client`, `Clients.AllExcept`, veya `Clients.Group` benzetimini yapmak için `Clients.Caller`, `Clients.Others`, veya `Clients.OthersInGroup`. Aşağıdaki örnekte, `MoveShapeHub` sınıfı için bağlantı kimliği geçirir `Broadcaster` sınıfı böylece `Broadcaster` sınıfı benzetimini `Clients.Others`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Grup üyeliğini yönetme

Bir Hub sınıfta yaptığınız gibi grupları yönetmek için aynı seçeneğiniz vardır.

- Bir gruba istemci Ekle

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- Bir istemci bir gruptan kaldırma

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Hub ardışık düzen özelleştirme

SignalR Hub ardışık düzenine kendi kodunuzu eklemenizi sağlar. Aşağıdaki örnek, istemci ve istemci üzerinde çağrılan giden yöntem çağrısı alınan gelen her yöntem çağrısı günlüklerini özel bir Hub ardışık düzen modülü gösterir:

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

Aşağıdaki kod *Global.asax* dosyayı Hub ardışık düzeninde çalıştırmak için modülü kaydeder:

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

Geçersiz kılabilirsiniz birçok farklı yöntem vardır. Tam bir listesi için bkz: [HubPipelineModule yöntemleri](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).
