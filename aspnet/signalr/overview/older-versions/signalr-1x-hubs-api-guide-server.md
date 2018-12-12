---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: ASP.NET SignalR Hubs API Kılavuzu - sunucu (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Bu belge, SignalR sürüm 1.1, kod örnekleri demonstratin ile ASP.NET SignalR hub'ları API sunucu tarafı programlama için bir giriş sağlar...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: a51a2077e0b6cde80bc679e3a310c0c804d19d68
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288033"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a>ASP.NET SignalR Hubs API Kılavuzu - sunucu (SignalR 1.x)
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Bu belge, sunucu tarafı ASP.NET SignalR hub'ları API sürüm 1.1, SignalR için genel seçenekleri gösteren kod örnekleri ile programlamaya giriş sağlar.
> 
> SignalR hub'ları API, bir sunucuya bağlanan istemcilerin ve istemcilerin sunucuya uzaktan yordam çağrısı (RPC) oluşturmanıza olanak sağlar. Sunucu kodu, istemciler tarafından çağrılabilen yöntemleri tanımlamak ve bir istemcide çalışmasına yöntemler çağırır. İstemci kodu sunucudan çağıran yöntemleri tanımlamak ve sunucu üzerinde çalışan yöntemleri çağırın. SignalR tüm istemci-sunucu tesisat sizin için üstlenir.
> 
> SignalR kalıcı bağlantı adlı bir alt düzey API'si de sunar. SignalR hub'ları ve kalıcı bağlantılar için giriş veya tam bir SignalR uygulamanın nasıl oluşturulacağını gösteren bir öğretici için bkz: [SignalR çalışmaya başlama -](index.md).


## <a name="overview"></a>Genel Bakış

Bu belgede aşağıdaki bölümler yer alır:

- [SignalR yol kaydetmek ve SignalR seçeneklerini yapılandırma](#route)

    - [/Signalr URL'si](#signalrurl)
    - [SignalR seçeneklerini yapılandırma](#options)
- [Oluşturma ve Hub sınıflarını kullanma](#hubclass)

    - [Hub nesne yaşam süresi](#transience)
    - [JavaScript istemcilerinin Hub adları camel casing](#hubnames)
    - [Birden çok hub'ları](#multiplehubs)
- [İstemciler çağırabileceğiniz Hub sınıfı yöntemleri tanımlama](#hubmethods)

    - [JavaScript istemcilerinin yöntemi adları camel casing](#methodnames)
    - [Zaman zaman uyumsuz olarak çalıştırmak için](#asyncmethods)
    - [Aşırı yüklemeler tanımlama](#overloads)
- [İstemci Hub sınıfı yöntemleri çağırma](#callfromhub)

    - [Hangi istemcilerin seçerek RPC alırsınız](#selectingclients)
    - [Yöntem adları için derleme zamanı doğrulama](#dynamicmethodnames)
    - [Ad eşleştirme büyük küçük harf duyarsız yöntemi](#caseinsensitive)
    - [Zaman uyumsuz yürütme](#asyncclient)
- [Hub sınıftan grup üyeliğini yönetme](#groupsfromhub)

    - [Ekleme ve kaldırma yöntemlerinin, zaman uyumsuz yürütme](#asyncgroupmethods)
    - [Grup üyeliği kalıcılığı](#grouppersistence)
    - [Tek kullanıcı grupları](#singleusergroups)
- [Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl](#connectionlifetime)

    - [OnConnected OnDisconnected ve OnReconnected olduğunda çağırılır](#onreconnected)
    - [Arayan durumu değil doldurulur](#nocallerstate)
- [Bağlam özelliği istemci bilgilerini alma](#contextproperty)
- [Nasıl durumu Hub sınıfına ve istemciler arasında geçirme](#passstate)
- [Hub sınıfında hatalarını işleme](#handleErrors)
- [İstemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetme](#callfromoutsidehub)

    - [İstemci yöntemleri çağırma](#callingclientsoutsidehub)
    - [Grup üyeliğini yönetme](#managinggroupsoutsidehub)
- [İzlemeyi etkinleştirme](#tracing)
- [Hub ardışık düzeni özelleştirme](#hubpipeline)

Program istemcilere hakkında daha fazla belge için aşağıdaki kaynaklara bakın:

- [SignalR hub API Kılavuzu - JavaScript istemcisi](index.md)
- [SignalR hub API Kılavuzu - .NET istemcisi](index.md)

API başvuru konularına bağlar API .NET 4.5 sürümü var. .NET 4 kullanıyorsanız, bkz. [API konuları .NET 4 sürümünü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a>SignalR yol kaydetmek ve SignalR seçeneklerini yapılandırma

İstemciler, Hub'ınıza bağlanmak için kullanacağı rota tanımlamak için çağrı [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) uygulama başlatıldığında yöntemi. `MapHubs` olan bir [genişletme yöntemi](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) için `System.Web.Routing.RouteCollection` sınıfı. Aşağıdaki örnek, bir SignalR hub'ları yolun tanımlamak gösterilmektedir *Global.asax* dosya.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

Bir ASP.NET MVC uygulaması için SignalR işlevselliği ekliyorsanız, SignalR rota diğer rotaların önce eklendiğinden emin olun. Daha fazla bilgi için [Öğreticisi: SignalR ve MVC 4 ile çalışmaya başlama](index.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>/Signalr URL'si

Varsayılan olarak, istemciler, Hub'ınıza bağlanmak için kullanacağı yönlendirme URL'si olan "/ signalr". (Bu URL için otomatik olarak oluşturulan JavaScript dosyası "/ signalr/hubs" URL ile karıştırmayın. Oluşturulan proxy hakkında daha fazla bilgi için bkz: [SignalR Hubs API Kılavuzu - JavaScript istemcisi - oluşturulan proxy ve sizin için yaptığı](index.md).)

Bu temel URL için SignalR kullanılamaz hale olağandışı durumlar olabilir; Örneğin, projenizde adlı bir klasöre sahip *signalr* ve adını değiştirmek istemiyorsanız. Bu durumda, aşağıdaki örneklerde gösterildiği gibi temel URL'sini değiştirebilirsiniz (Değiştir "/ signalr" örnek kodda, istenen URL ile).

**URL'yi belirten sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

**JavaScript istemci URL'si (ile oluşturulan proxy) belirten bir kod**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

**(Olmadan oluşturulan proxy) URL'sini belirtir, JavaScript istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

**URL'sini belirtir, .NET istemci kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>SignalR seçeneklerini yapılandırma

Overloads biri `MapHubs` yöntemini etkinleştirmek, özel bir URL, bir özel bağımlılık çözümleyiciyi ve aşağıdaki seçenekleri belirtin:

- Etki alanları arası çağrılar tarayıcı istemcilerinden gelen etkinleştirin.

    Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantı `http://contoso.com/signalr`. Varsa sayfasından `http://contoso.com` için bir bağlantı kurar `http://fabrikam.com/signalr`, diğer bir deyişle etki alanları arası bağlantı. Güvenlik nedenleriyle, etki alanları arası bağlantıları varsayılan olarak devre dışıdır. Daha fazla bilgi için [ASP.NET SignalR Hubs API Kılavuzu - JavaScript istemcisi - etki alanları arası bağlantı kurmak nasıl](index.md).
- Ayrıntılı hata iletilerini etkinleştirin.

    Hatalar oluştuğunda, SignalR varsayılan davranışını istemciler için ne hakkında ayrıntılar olmadan bir bildirim iletisi göndermektir. Kötü amaçlı kullanıcıların uygulamanızı saldırıları bilgileri kullanmak mümkün olabilir çünkü istemciler için ayrıntılı hata bilgilerini göndermesini üretimde önerilmez. Sorun giderme için geçici olarak daha bilgilendirici hata raporlamasını etkinleştirmek için bu seçeneği kullanabilirsiniz.
- Otomatik olarak oluşturulan JavaScript proxy'si dosyaları devre dışı bırakın.

    Varsayılan olarak, yanıt URL'sine "/ signalr/hubs" Hub sınıflarınızı için proxy ile bir JavaScript dosyası oluşturulur. JavaScript proxy'leri kullanmak istemiyorsanız veya bu dosyayı el ile oluşturmanız ve istemcilerinizin fiziksel bir dosyaya başvurmak istiyorsanız, proxy oluşturma devre dışı bırakmak için bu seçeneği kullanabilirsiniz. Daha fazla bilgi için [SignalR Hubs API Kılavuzu - JavaScript istemcisi - fiziksel dosya oluşturma SignalR için oluşturulan proxy](index.md).

Aşağıdaki örnek, bir çağrıda SignalR bağlantı URL'si ve bu seçenekleri belirtmek gösterilmektedir `MapHubs` yöntemi. Özel bir URL belirtmek için Değiştir "/ signalr" örnekte URL'siyle kullanmak istiyorsunuz.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Oluşturma ve Hub sınıflarını kullanma

Bir Hub oluşturmak için türetilen bir sınıf oluşturma [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). Aşağıdaki örnek, basit bir Hub sınıfı için bir sohbet uygulaması gösterir.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

Bu örnekte, bir bağlı istemci çağırabilirsiniz `NewContosoChatMessage` yöntemi ve yaptığında, alınan verilerin bağlanan tüm istemciler için yayımladınız.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Hub nesne yaşam süresi

Hub sınıfının örneği yok ya da sunucu üzerindeki kendi koddan yöntemlerinin çağrılması; Tüm bunları sizin için SignalR hub'ları işlem hattı tarafından gerçekleştirilir. SignalR hub'ı sınıfınıza yeni bir örneğini ne zaman bir istemci bağlanır, bağlantısı kesildiğinde veya sunucu için bir yöntem çağrısı yapar gibi bir Hub işlemin işlemek için gereken her zaman oluşturur.

Hub sınıfının örneklerini geçici olduğu için bir yöntem çağrısından sonraki durumunu korumak üzere kullanamazsınız. Her sunucunun bir yöntem çağrısının bir istemciden Hub sınıfı işlemlerinizi yeni bir örneğini iletiyi alır. Birden fazla bağlantı ve yöntem çağrıları arasında durumu korumak için Hub sınıfına veya farklı bir sınıf türünden türemez bir veritabanı veya statik bir değişken gibi bazı başka bir yöntem kullanın `Hub`. Bellekteki verileri devam ediyorsa, uygulama etki alanı geri dönüştürüldüğünde Hub sınıfında statik bir değişken gibi bir yöntem kullanarak verileri kaybolur.

Kendi koddan Hub sınıfı dışında çalışan istemciler için iletileri göndermek istiyorsanız bir Hub örneği oluşturarak bunu yapamazsınız, ancak Hub sınıfınız için bir başvuru SignalR bağlam nesnesi alarak yapabilirsiniz. Daha fazla bilgi için [istemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetmek nasıl](#callfromoutsidehub) bu konuda.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>JavaScript istemcilerinin Hub adları camel casing

Varsayılan olarak, JavaScript istemcilerinin, sınıf adı ortası büyük küçük harfleri sürümünü kullanarak hub'larına bakın. Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar. Önceki örneği olarak adlandırılır `contosoChatHub` JavaScript kod.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemcisi**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

İstemcilerin kullanın, eklemek farklı bir ad belirlemek istiyorsanız `HubName` özniteliği. Kullandığınızda, bir `HubName` özniteliği, için JavaScript istemcilerde ortası büyük harf adı bir değişiklik yoktur.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemcisi**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Birden çok hub'ları

Bir uygulamada birden fazla Hub sınıfı tanımlayabilirsiniz. Bunu yaptığınızda, paylaşılan bir bağlantısı ancak grupları ayrı:

- Tüm istemciler, hizmetiniz ile SignalR bağlantı kurmak için aynı URL'yi kullanır ("/ signalr" ya da bir belirttiyseniz, özel URL), hizmet tarafından tanımlanan ve bağlantı tüm hub'ları için kullanılır.

    Tek bir sınıfta tüm Hub işlevselliği tanımlamak için kıyasla çok sayıda hub için bir performans farkı yoktur.
- Tüm hub'ları için aynı HTTP isteği bilgilerini edinin.

    Tüm hub'ları aynı bağlantıyı paylaşmak olduğundan, sunucunun aldığı yalnızca HTTP isteği ne SignalR bağlantı kurar özgün HTTP isteği gelen bilgilerdir. Bir sorgu dizesi belirterek bilgi istemciden sunucuya geçirmek için bağlantı isteğini kullanırsanız, farklı sorgu dizeleri için farklı hub'lar sağlayamaz. Tüm hub'ları aynı bilgileri alır.
- Tek bir dosyada, tüm hub'ları proxy'lerini oluşturulan JavaScript proxy'leri dosyasını içerir.

    JavaScript proxy'si hakkında daha fazla bilgi için bkz. [SignalR Hubs API Kılavuzu - JavaScript istemcisi - oluşturulan proxy ve sizin için yaptığı](index.md).
- Hub'ları grupları tanımlanır.

    Adlandırılmış alt kümelerini bağlı istemciler için yayın gruplar SignalR öğesinde tanımlayabilirsiniz. Grupları, her Hub için ayrı olarak korunur. Örneğin, "Yöneticiler" adlı bir grup istemciler için bir dizi verilebilir, `ContosoChatHub` sınıfı ve aynı ada istemciler için farklı bir dizi bakın, `StockTickerHub` sınıfı.

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>İstemciler çağırabileceğiniz Hub sınıfı yöntemleri tanımlama

İstemciden çağrılabilir olmasını istediğiniz Hub üzerindeki bir yöntem kullanıma sunmak için aşağıdaki örneklerde gösterildiği gibi genel bir yöntem bildirin.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Dönüş türü ve parametreleri, tüm C# yönteminde olduğu gibi karmaşık türler ve diziler de dahil olmak üzere belirtebilirsiniz. Parametreleri almak veya çağırana döndürmesi herhangi bir veri istemci ve sunucu arasında JSON'ı kullanarak iletilir ve SignalR bağlama karmaşık nesnelerin ve nesne dizileri otomatik olarak işler.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>JavaScript istemcilerinin yöntemi adları camel casing

Varsayılan olarak, JavaScript istemcilerinin, Hub yöntemleri için bir yöntem adı ortası büyük küçük harfleri sürümünü kullanarak bakın. Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemcisi**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

İstemcilerin kullanın, eklemek farklı bir ad belirlemek istiyorsanız `HubMethodName` özniteliği.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemcisi**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Zaman zaman uyumsuz olarak çalıştırmak için

Yöntemi uzun süre çalışan olması veya çalışmaya sahipse, Bekliyor, bir veritabanı araması veya bir web hizmeti çağrısı gibi ilgili, döndürerek Hub yönteminin zaman uyumsuz hale bir [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (yerine `void` döndürür) veya [ Görev&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) nesne (yerine `T` dönüş türü). Döndüğünüzde bir `Task` yöntemi, SignalR nesneden bekler `Task` tamamlamak için ve istemci yöntem çağrısında nasıl kod içinde herhangi bir fark olması, sarmalanmış halden sonucu istemciye geri gönderir.

Bir Hub yöntemini olmasını zaman uyumsuz WebSocket taşıma kullandığında bağlantıya engel önler. Hub yönteminin tamamlanana kadar bir Hub yöntemini zaman uyumlu olarak yürütülür ve taşıma WebSocket olduğunda, aynı istemciden hub yöntemlerine yönelik sonraki çağrılarını engellenir.

Aynı yöntem eşzamanlı çalışacak biçimde kodlanmış veya zaman uyumsuz olarak çalışan iki sürümden çağırmak için JavaScript istemci kodu ardından aşağıdaki örnekte gösterilmiştir.

**Zaman uyumlu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

**Zaman uyumsuz - ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Oluşturulan proxy kullanarak JavaScript istemcisi**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

ASP.NET 4.5 içinde zaman uyumsuz yöntemler kullanma hakkında daha fazla bilgi için bkz. [kullanarak ASP.NET MVC 4'te zaman uyumsuz yöntemleri](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Aşırı yüklemeler tanımlama

Yöntemi için aşırı yüklemeleri tanımlamak istiyorsanız, her aşırı yükleme parametre sayısı farklı olması gerekir. Farklı parametre türleri belirterek bir aşırı ayırt Hub sınıfınıza derleyeceği ancak SignalR hizmeti, çağrı aşırı istemciler çalıştığınızda çalışma zamanında bir özel durum oluşturur.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>İstemci Hub sınıfı yöntemleri çağırma

İstemcisi, sunucudan yöntemleri çağırmak için kullanın `Clients` Hub sınıfınızın bir yöntemde bir özellik. Aşağıdaki örnek, çağıran sunucu kodu gösterir `addNewMessageToPage` tüm bağlı istemcileri ve istemci kodu, bir JavaScript istemci yöntemi tanımlar.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

**Oluşturulan proxy kullanarak JavaScript istemcisi**

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

İstemci yöntemden dönüş değeri alınamıyor; söz dizimi gibi `int x = Clients.All.add(1,1)` çalışmıyor.

Karmaşık türler ve diziler için parametreleri belirtebilirsiniz. Aşağıdaki örnek bir yöntem parametresi istemci bir karmaşık tür geçirir.

**Karmaşık bir nesne kullanarak bir istemci yöntemini çağıran sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

**Karmaşık bir nesne tanımlayan bir sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

**Oluşturulan proxy kullanarak JavaScript istemcisi**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Hangi istemcilerin seçerek RPC alırsınız

İstemciler özelliği döndürür bir [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) hangi istemcilerin RPC alırsınız belirtmek için çeşitli seçenekler sağlayan nesne:

- Bağlanan tüm istemciler.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- Çağıran istemci yalnızca.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- Çağıran istemci dışındaki tüm istemcilerin.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- Bağlantı kimliği ile tanımlanan belirli bir istemci

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    Bu örnek `addContosoChatMessageToPage` çağıran istemci hakkında ve kullanmakla aynı etkiye sahip `Clients.Caller`.
- Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- Belirli bir grubun tüm bağlı istemcileri.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- Belirtilen istemcilerin bağlantı kimliği ile tanımlanan dışında belirtilen gruptaki tüm bağlı istemcileri

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- Belirli bir grubun tüm bağlı istemcileri çağıran istemci dışındaki.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Yöntem adları için derleme zamanı doğrulama

Belirttiğiniz yöntem adı, IntelliSense veya derleme zamanı doğrulamasını yoktur anlamına gelen dinamik bir nesne olarak yorumlanır. İfade, çalışma zamanında değerlendirilir. Yöntem çağrısının yürütüldüğünde, SignalR yöntem adı ve parametre değerlerini istemciye gönderir ve istemci bir yöntemi varsa, adla eşleşen, yöntemi çağrılır ve parametre değerlerini, kendisine geçirilir. Eşleşen hiçbir yöntemi istemcide bulunursa, herhangi bir hata ortaya çıkar. Bir istemci yöntemi çağırdığınızda, arka planda istemciye SignalR ileten veri biçimi hakkında daha fazla bilgi için bkz [signalr'a giriş](index.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Ad eşleştirme büyük küçük harf duyarsız yöntemi

Yöntem adı ile eşleşen büyük küçük harfe duyarlıdır. Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütülür `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, veya `addContosoChatMessageToPage` istemci üzerinde.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Zaman uyumsuz yürütme

Çağıran, yöntemi zaman uyumsuz olarak yürütür. Bir istemci için bir yöntem çağrısının hemen sonraki kod satırlarını siz belirtmediğiniz sürece, istemciye veri aktarımı tamamlamak, SignalR için beklemenize gerek kalmadan yürütecek sonra gelen kodu yöntemi tamamlanmasını beklemeniz gerekir. Aşağıdaki kod örnekleri iki istemci yöntemleri ardışık olarak yürütmek gösterilmektedir, kullanarak .NET 4.5 içinde çalışan kod, diğeri kullanarak .NET 4'te çalışan kod.

**.NET 4.5 örneği**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

**.NET 4 örneği**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

Kullanırsanız `await` veya `ContinueWith` sonraki satırlık bir kod yürütülmeden önce bir istemci yöntemi bitene kadar beklemek için gelmez sonraki satırlık bir kod yürütülmeden önce istemcileri aslında iletiyi alır. Yalnızca bir istemci yöntem çağrısının "tamamlama" SignalR ileti göndermek için gereken her şey yapmış anlamına gelir. İstemcilerin ileti aldığı doğrulama gerekiyorsa, bu mekanizma kendiniz program gerekir. Örneğin, kod bir `MessageReceived` yöntemi Hub hem de `addContosoChatMessageToPage` , çağırın istemcide yöntemi `MessageReceived` , yaptıktan sonra iş istemcide gerçekleştirmek için ihtiyaç. İçinde `MessageReceived` hub'ı gerçek istemci alma ve işleme özgün yöntem çağrısının hangi iş bağlıdır, bunu yapabilirsiniz.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Yöntem adı bir dize değişkeni kullanma

Cast yöntemi adı bir dize değişkeni kullanarak bir istemci yöntem çağırmak istiyorsanız `Clients.All` (veya `Clients.Others`, `Clients.Caller`, vs.) için `IClientProxy` ve sonra çağrı [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Hub sınıftan grup üyeliğini yönetme

Signalr'da gruplarla, bağlı istemciler belirtilen alt kümelerine yayın iletileri için bir yöntem sağlar. Bir grupta herhangi bir sayıda istemciler olabilir ve istemci grupları herhangi bir sayıda üyesi olabilir.

Grup üyeliğini yönetmek için kullandığınız [Ekle](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ve [Kaldır](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) tarafından sağlanan yöntemleri `Groups` Hub sınıfın özelliği. Aşağıdaki örnekte gösterildiği `Groups.Add` ve `Groups.Remove` istemci kodu tarafından çağrılan Hub yöntemlerinde kullanılan yöntemleri, onları çağıran JavaScript istemci kodu tarafından izlenen.

**Sunucu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

**Oluşturulan proxy kullanarak JavaScript istemcisi**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

Açıkça grupları oluşturmanız gerekmez. Etkin bir grup çağrıda adını belirttiğiniz ilk kez otomatik olarak oluşturulur `Groups.Add`, ve bu üyelik son bağlantı kaldırdığınızda silinir.

Bir grup üyeliği listesinin veya grupların listesini almak için hiçbir API yoktur. SignalR istemcileri ve gruplara göre iletiler gönderen bir [pub/sub modeli](http://en.wikipedia.org/wiki/Publish/subscribe), ve sunucu grupları veya grup üyeliklerinin listesi korumaz. Bir web grubu için bir düğüm eklediğinizde, yeni bir düğüme dağıtılmasını SignalR tutar herhangi bir durum olduğundan bu ölçeklenebilirliği en üst düzeye yardımcı olur.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Ekleme ve kaldırma yöntemlerinin, zaman uyumsuz yürütme

`Groups.Add` Ve `Groups.Remove` zaman uyumsuz bir yöntem yürütülemez. Bir istemci bir gruba ekleyin ve hemen bir ileti grubunu kullanarak istemciye göndermek istiyorsanız, emin olmak sahip `Groups.Add` yöntemi önce tamamlanır. Aşağıdaki kod örnekleri, .NET 4.5 ve .NET 4'te çalışan kod kullanarak bir tane çalışan kod kullanarak bunun nasıl yapılacağını göster

**.NET 4.5 örneği**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

**.NET 4 örneği**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Grup üyeliği kalıcılığı

SignalR bağlantıları izler, kullanıcıları değil, dolayısıyla bir kullanıcı kullanıcı her bağlandığında bağlantı aynı grupta olmasını istediğiniz, çağırmak zorunda `Groups.Add` her zaman kullanıcı yeni bir bağlantı kurar.

Geçici bağlantı kaybı sonra bazen SignalR bağlantı otomatik olarak geri yükleyebilirsiniz. Bu durumda, yeni bir bağlantı kurmadan aynı bağlantıyı SignalR geri yüklüyor ve bu nedenle istemcinin Grup üyeliğini otomatik olarak geri. Bağlantı durumu grup üyelikleri de dahil olmak üzere, her istemci için istemcinin gidiş dönüşlü geçici kesme sunucu yeniden başlatma veya hata sonucu olsa bile mümkün olmasıdır. Bir sunucu arıza ve bağlantı zaman aşımına uğramadan önce yeni bir sunucu tarafından değiştirilir, istemci otomatik olarak yeni sunucuya yeniden ve üyesi olduğu grupları'nı yeniden kaydolun.

Bir bağlantı, bağlantı kaybı sonra otomatik olarak geri yüklenemez veya bağlantı zaman aşımına uğradığında veya (örneğin, bir tarayıcı için yeni bir sayfa gittiğinde) istemci kestiğinde, grup üyeliği kaybedilir. Kullanıcı bir sonraki bağlanışında, yeni bir bağlantı olacaktır. Aynı kullanıcı yeni bir bağlantı kurduğunda grup üyeliklerini korumasına kullanıcılar ve gruplar ilişkileri izlemek ve grup üyelikleri kullanıcı yeni bir bağlantı kurar ve her zaman geri yüklemek uygulamanız gerekir.

Bağlantılar ve tutarsızlıklara hakkında daha fazla bilgi için bkz. [Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl](#connectionlifetime) bu konuda.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Tek kullanıcı grupları

SignalR genellikle kullanan uygulamalar, hangi kullanıcının bir ileti gönderdi ve hangi kullanıcının bir ileti almalıdır öğrenmek için kullanıcılar ve bağlantılar arasındaki ilişkileri izlemek zorunda. Grupları iki yaygın olarak kullanılan desenlerden birini yapmak için kullanılır.

- Tek kullanıcı grupları.

    Kullanıcı adı grup adı belirtin ve kullanıcı bağlanır veya yeniden her zaman geçerli bağlantı kimliği gruba ekleyin. Kullanıcıya ileti göndermek için Grup gönderin. Bu yöntem bir dezavantajı, gruba kullanıcı çevrimiçi veya çevrimdışı olup olmadığını öğrenmek için bir yol sağlamaz olmasıdır.
- Kullanıcı adları ve bağlantı kimlikleri arasındaki ilişkilendirmeleri izleyin.

    Her bir kullanıcı adı ve bir veya daha fazla bağlantı kimlikleri arasında bir ilişki bir sözlük veya veritabanında depolamak ve depolanan veriler her zaman kullanıcı bağlandığında veya bağlantısı kesildiğinde güncelleştirin. Kullanıcıya ileti göndermek için bağlantı kimliği belirtin. Bu yöntem bir dezavantajı, daha fazla bellek alır olmasıdır.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl

Bağlantı ömrü olaylarını işlemek için normal bir kullanıcı veya bağlı olup olmadığını izler ve kullanıcı adları ve bağlantı kimlikleri arasındaki ilişkiyi izlemek için nedenleridir. İstemciler bağlanın veya bağlantıyı kesin zaman kendi kodunuzu çalıştırmak için geçersiz kılma `OnConnected`, `OnDisconnected`, ve `OnReconnected` aşağıdaki örnekte gösterildiği gibi hub'ın sanal yöntemler sınıf.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>OnConnected OnDisconnected ve OnReconnected olduğunda çağırılır

Bir tarayıcı yeni bir sayfaya gider her seferinde yeni bir bağlantı kurulması SignalR yürütülecek anlamına gelir sahip `OnDisconnected` yöntemi arkasından `OnConnected` yöntemi. Yeni bir bağlantı kurulduğunda SignalR her zaman yeni bir bağlantı kimliği oluşturur.

`OnReconnected` Olduğunda geçici kesme SignalR otomatik olarak, ne zaman kablo geçici olarak bağlantısı kesilir ve bağlantı zaman aşımına uğramadan önce yeniden bağlantı kuruldu gibi kurtarabileceğiniz bağlantılar yöntemi çağrılır. `OnDisconnected` İstemcinin bağlantısı kesildi ve SignalR olamaz otomatik olarak yeniden, ne zaman yeni bir sayfaya bir tarayıcı gider gibi yöntemi çağrılır. Bu nedenle, olası belirli bir istemcinin olayları dizisidir `OnConnected`, `OnReconnected`, `OnDisconnected`; veya `OnConnected`, `OnDisconnected`. Sıra görmezsiniz `OnConnected`, `OnDisconnected`, `OnReconnected` belirli bir bağlantı için.

`OnDisconnected` Değil yönteminden ne zaman bir sunucu arıza gibi bazı senaryolarda veya uygulama etki alanı geri alır. Başka bir sunucuya gelinceye veya uygulama etki alanı, geri dönüşüm tamamlandıktan, bazı istemciler bağlanın ve yangın mümkün olabilir `OnReconnected` olay.

Daha fazla bilgi için [anlama ve signalr'da bağlantı ömrü olaylarını işleme](index.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Arayan durumu değil doldurulur

Bağlantı ömrü olay işleyicisi yöntemleri içine girdiğiniz herhangi bir durumu anlamına sunucudan adlı `state` istemci üzerinde nesne değil doldurulacak içinde `Caller` sunucudaki özelliği. Hakkında bilgi için `state` nesne ve `Caller` özelliği bkz [Hub sınıfına ve istemciler arasında durumunu nasıl](#passstate) bu konuda.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Bağlam özelliği istemci bilgilerini alma

İstemcisi hakkında bilgi almak için kullanın `Context` Hub sınıfın özelliği. `Context` Özelliği döndürür bir [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) aşağıdaki bilgilere erişim sağlayan nesne:

- Çağıran istemcinin bağlantı kimliği.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    Bağlantı kimliği (değeri kendi kodunuzda belirtemezsiniz) SignalR tarafından atanan bir GUID'dir. Her bağlantı ve uygulamanızda birden çok hub'a varsa tüm hub'ları tarafından kullanılan kimliği aynı bağlantı için bir bağlantı kimliği yok.
- HTTP üst bilgisi verileri.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    HTTP üst bilgiler de alabilirsiniz `Context.Headers`. Aynı şeyi birden çok başvuru nedeni `Context.Headers` ilk olarak oluşturulduğu `Context.Request` özelliği daha sonra eklenen ve `Context.Headers` geriye dönük uyumluluk için tutulmaktadır.
- Dize verileri sorgulayın.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    Sorgu dizesi verileri da edinebilirsiniz `Context.QueryString`.

    Bu özelliği alma sorgu dizesi SignalR bağlantısı HTTP isteği ile kullanılan paroladır. İstemci istemci hakkındaki verileri istemciden sunucuya geçirmek için kullanışlı bir yoldur bağlantı yapılandırarak, sorgu dizesi parametreleri ekleyebilirsiniz. Aşağıdaki örnek, oluşturulan proxy kullandığınızda, JavaScript istemci olarak bir sorgu dizesi eklemek için yollarından biri gösterilmektedir.

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    Sorgu dizesi parametreleri ayarlama hakkında daha fazla bilgi için bkz. API kılavuzları için [JavaScript](index.md) ve [.NET](index.md) istemciler.

    SignalR tarafından dahili olarak kullanılan bazı diğer değerler yanı sıra sorgu dize verileri, bağlantı için kullanılan aktarım yöntemi bulabilirsiniz:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    Değerini `transportMethod` "webSockets", "serverSentEvents", "foreverFrame" veya "longPolling" olacaktır. Bu değer iade gerçekleştiriyorsanız `OnConnected` olay işleyicisi yönteminde, bazı senaryolarda ilk bağlantı için son anlaşılan taşıma yöntemini değil bir taşıma değeri alabilirsiniz. Bu durumda yöntem bir özel durum oluşturur ve daha sonra tekrar son taşıma yöntemi oluşturulduğunda çağrılır.
- Tanımlama bilgileri.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Tanımlama bilgilerini de alabilirsiniz `Context.RequestCookies`.
- Kullanıcı bilgileri.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- İstek için HttpContext nesnesi:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    Almak yerine bu yöntemi kullanmak `HttpContext.Current` almak için `HttpContext` SignalR bağlantı nesnesi.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Nasıl durumu Hub sınıfına ve istemciler arasında geçirme

İstemci proxy sağlayan bir `state` içinde depolayabileceğiniz her yöntem çağrısının ile sunucuya aktarılması istediğiniz veri nesnesi. Sunucu üzerinde bu verilerine erişebilir `Clients.Caller` istemciler tarafından çağrılan Hub yöntemlerini bir özellik. `Clients.Caller` Özelliği için bağlantı ömrü olay işleyicisi yöntemleri doldurulmamışsa `OnConnected`, `OnDisconnected`, ve `OnReconnected`.

Oluşturma veya güncelleştirme verilerinde `state` nesne ve `Clients.Caller` özelliği, her iki yönde de çalışır. Değerleri sunucusunda güncelleştirebilirsiniz ve bunlar istemciye geçirilir.

Aşağıdaki örnek, iletilmesi için her yöntem çağrısının ile sunucu durumunu depolayan JavaScript istemci kodu gösterir.

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

Aşağıdaki örnek bir .NET istemci eşdeğer kod gösterir.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

Hub sınıfınızda, bu verilerine erişebilir `Clients.Caller` özelliği. Aşağıdaki örnek, önceki örnekte başvurulan durumunu alır. kod gösterir.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> Kalıcı hale getirme durumu için bu düzenek itibaren her şeyi içine girdiğiniz büyük miktarlarda veri için tasarlanmamıştır `state` veya `Clients.Caller` özellik gidiş dönüşlü ile her yöntem çağırma. Kullanıcı adlarını veya sayaçları gibi küçük öğeler için kullanışlıdır.


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Hub sınıfında hatalarını işleme

Hub sınıfı yöntemlerinde meydana gelen hataları işlemek için aşağıdakilerden birini veya her ikisi de aşağıdaki yöntemlerden birini kullanın:

- Try-catch bloğu içinde yöntemi kodunuzu sarın ve özel durum nesnesi oturum açın. Hata ayıklama amacıyla istemciye bir özel durum gönderebilir, ancak güvenlik için üretim istemciler için ayrıntılı bilgi gönderme nedeniyle önerilmez.
- İşleme bir hub'ları işlem hattı modülünüzü oluşturmak [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) yöntemi. Aşağıdaki örnek, hatalar, modül hub ardışık düzene ekler Global.asax içindeki kod tarafından izlenen günlüklerini bir işlem hattı modül gösterir.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

Hub ardışık düzen modüllerine hakkında daha fazla bilgi için bkz: [hub ardışık düzeni özelleştirildiği nasıl](#hubpipeline) bu konuda.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>İzlemeyi etkinleştirme

System.diagnostics öğesi sunucu-tarafı izlemeyi etkinleştirmek için Web.config dosyasına, bu örnekte gösterildiği gibi ekleyin:

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

Visual Studio'da uygulamayı çalıştırdığınızda, günlükleri görüntüleyebilirsiniz **çıkış** penceresi.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>İstemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetme

İstemci Hub sınıfınıza daha farklı bir sınıftaki yöntemleri çağırmak için hub'ı için SignalR bağlam nesnesi bir başvuru almak ve, istemcide yöntemlerini çağıran veya grupları yönetmek için kullanın.

Aşağıdaki örnek `StockTicker` sınıfı bağlam nesnesini alır, sınıfının bir örneğini depolar, statik özellik bir sınıf örneğini depolar ve çağırmak için singleton sınıfı örneğinden bağlamı kullanır `updateStockPrice` istemciler yöntemi adlı bir Hub'ına bağlı `StockTickerHub`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

Uzun süreli bir nesne bağlamı birden çok kez kullanmanız gerekiyorsa, başvuru kez almak ve bunun yerine her zaman yeniden başlama kaydedin. Bir kez bağlamı alma SignalR istemcilere içinde ve Hub yöntemlerinizi istemci yöntem çağrıları yapmak aynı sıradaki iletiler gönderir sağlar. SignalR bağlamı için bir hub'ı kullanmayı gösteren bir öğretici için bkz. [ASP.NET SignalR ile sunucu yayını](index.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>İstemci yöntemleri çağırma

Hangi istemcilerin RPC alırsınız belirtebilirsiniz, ancak bir Hub sınıftan çağrısı yaparken daha az seçeneğiniz vardır. Bunun nedeni herhangi bir yöntem gibi geçerli bir bağlantı kimliği bilgisi gerektiren şekilde bağlamı bir istemci belirli bir çağrıdan ilişkilendirilmiş olmasıdır `Clients.Others`, veya `Clients.Caller`, veya `Clients.OthersInGroup`, kullanılabilir değil. Aşağıdaki seçenekler mevcuttur:

- Bağlanan tüm istemciler.

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- Bağlantı kimliği ile tanımlanan belirli bir istemci

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- Belirli bir grubun tüm bağlı istemcileri.

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- Bağlantı kimliği ile tanımlanan, belirtilen istemcilerin dışında belirtilen gruptaki tüm bağlı istemcileri

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

Hub sınıfınızda yöntemlerinden Hub olmayan sınıfınıza çağırıyorsanız, geçerli bağlantı kimliği geçirin ve ile kullanan `Clients.Client`, `Clients.AllExcept`, veya `Clients.Group` benzetimini yapmak için `Clients.Caller`, `Clients.Others`, veya `Clients.OthersInGroup`. Aşağıdaki örnekte, `MoveShapeHub` sınıfı için bağlantı kimliği geçirir `Broadcaster` sınıfı böylece `Broadcaster` sınıfı benzetimini yapmak `Clients.Others`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Grup üyeliğini yönetme

Bir Hub sınıfta olarak grupları yönetme için aynı seçeneklere sahip.

- Bir gruba istemci Ekle

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- Bir istemci bir gruptan kaldırma

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Hub ardışık düzeni özelleştirme

SignalR Hub ardışık düzende kendi kod eklemesini sağlar. Aşağıdaki örnek, istemci ve istemci üzerinde çağrılır giden yöntem çağrısının alınan gelen her yöntem çağrısının günlükleri özel bir Hub ardışık düzen modülü gösterir:

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

Aşağıdaki kod içinde *Global.asax* dosyayı, işlem hattında hub'ı çalıştırmak için modül kaydeder:

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

Geçersiz kılabilirsiniz birçok farklı yöntem vardır. Tam bir listesi için bkz. [HubPipelineModule yöntemleri](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
