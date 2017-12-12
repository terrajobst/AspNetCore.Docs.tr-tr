---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: "Öğretici: Sunucu yayın 2 SignalR ile | Microsoft Docs"
author: tdykstra
description: "Bu öğretici sunucu yayın işlevselliği sağlamak için ASP.NET SignalR 2 kullanan bir web uygulamasının nasıl oluşturulacağını gösterir. Sunucu yayın o commun anlama gelir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: cd800062e87c07a0ef1d8d3d32c910aaf3e683cc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>Öğretici: Sunucu yayın SignalR 2 ile
====================
tarafından [zel Dykstra](https://github.com/tdykstra), [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici sunucu yayın işlevselliği sağlamak için ASP.NET SignalR 2 kullanan bir web uygulamasının nasıl oluşturulacağını gösterir. Sunucu yayın istemcilere gönderilen iletişimleri sunucu tarafından başlatılan anlamına gelir. Bu senaryo, eşler arası senaryolar istemcilere iletişimler bir veya daha fazla istemcileri tarafından başlatılır, sohbet uygulamaları gibi daha farklı bir programlama yaklaşımı gerektirir.
> 
> Bu öğreticide oluşturacaksınız uygulama bir borsa sunucusu yayın işlevselliği için tipik bir senaryo benzetimini yapar.
> 
> Bu konuda, ilk olarak CAN Fletcher'dan tarafından yazıldı.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Bu öğretici ile Visual Studio 2012 kullanma
> 
> 
> Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:
> 
> - Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.
> - Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).
> - Web Platformu Yükleyicisi'nde arayın ve yükleyin **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**. Bu SignalR sınıfları için Visual Studio şablonları gibi yükleyecek **Hub**.
> - Bazı şablonlar (gibi **OWIN başlangıç sınıfı**); kullanılamaz bunlar için bunun yerine bir sınıf dosyası kullanın.
> 
> 
> ## <a name="tutorial-versions"></a>Eğitmen sürümleri
> 
> SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, düzenli aralıklarla göndermek"" istediğiniz gerçek zamanlı uygulamaları temsilcisidir bir borsa uygulaması oluşturun veya yayın, sunucudan bildirimleri bağlanan tüm istemciler için. Bu öğreticinin ilk bölümü baştan uygulama basitleştirilmiş bir sürümünü oluşturacaksınız. Öğretici kalanı ek özellikler içeren bir NuGet paketi yükleyin ve bu özellikleri için kod gözden geçirme.

Bu öğreticinin ilk bölümü oluşturacağınız uygulama hisse senedi verileri içeren bir kılavuz görüntüler.

![StockTicker ilk sürüm](tutorial-server-broadcast-with-signalr/_static/image1.png)

Düzenli aralıklarla sunucu rastgele hisse senedi fiyatları güncelleştirir ve tüm bağlı istemcileri güncelleştirmeleri iter. Tarayıcı sayıları ve sembolleri **değiştirme** ve  **%**  sütunları dinamik olarak değiştirme bildirimlere yanıt olarak sunucudan. Aynı URL'ye ek tarayıcılar açarsanız, hepsi aynı veri ve veri aynı değişiklikleri aynı anda gösterin.

Bu öğretici aşağıdaki bölümleri içerir:

- [Önkoşulları](#prerequisites)
- [Proje oluşturma](#createproject)
- [Sunucu kodu ayarlamanız](#server)
- [İstemci kodu ayarlamanız](#client)
- [Uygulamayı test etme](#test)
- [Günlük kaydını etkinleştir](#enablelogging)
- [Yükleme ve tam StockTicker örnek gözden geçirin](#fullsample)
- [Sonraki adımlar](#nextsteps)

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet paketini bir Visual Studio projesi benzetimli örnek bir borsa uygulamayı yükler.

> [!NOTE]
> Uygulama oluşturma adımları boyunca çalışmaya istemiyorsanız, yeni bir boş bir ASP.NET Web uygulaması projesindeki SignalR.Sample paketi yükleyebilirsiniz. Bu öğreticide adımları uygularken olmadan NuGet paketi yüklerseniz **readme.txt dosyasında yönergeleri izlemelisiniz**. Paketi çalıştırmak için yüklenen pakette ConfigureSignalR yöntemini çağıran bir OWIN başlangıç sınıfı eklemeniz gerekir. OWIN başlangıç sınıfı eklemezseniz bir hata alırsınız.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce Visual Studio 2013 bilgisayarınızda yüklü olduğundan emin olun. Visual Studio sahip değilseniz, bkz: [ASP.NET indirmeleri](https://www.asp.net/downloads) ücretsiz Visual Studio 2013 Express alınamıyor.

<a id="createproject"></a>

## <a name="create-the-project"></a>Projeyi oluşturma

1. Gelen **dosya** menüsünde tıklatın **yeni proje**.
2. İçinde **yeni proje** iletişim kutusunda, genişletin **C#** altında **şablonları** seçip **Web**.
3. Seçin **ASP.NET boş Web uygulaması** şablonu, proje adı *SignalR.StockTicker*, tıklatıp **Tamam**.

    ![Yeni Proje iletişim kutusu](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. İçinde **yeni ASP.NET** proje penceresinin, bırakın **boş** 'ı tıklatın ve seçili **proje oluştur**.

    ![Yeni ASP proje iletişim kutusu](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Sunucu kodu ayarlamanız

Bu bölümde, sunucu üzerinde çalışan bir kod ayarlayın.

### <a name="create-the-stock-class"></a>Hisse senedi sınıfı oluşturma

Depolamak ve stok hakkında bilgi aktarmak için kullanacağınız hisse senedi model sınıfı oluşturarak başlayın.

1. Proje klasöründe yeni bir sınıf dosyası oluşturun, adlandırın *Stock.cs*ve ardından şablon kodu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    İstediğiniz hisse oluşturduğunuzda, ayarlarız iki simge (örneğin, Microsoft MSFT) ve fiyat özelliklerdir. Diğer özellikler nasıl ve ne zaman fiyat ayarlama bağlıdır. Fiyat, ayarladığınız ilk kez değeri için DayOpen yayılan. Zaman fiyat, değişikliği ayarlamak ve YüzdeDeğişiklikleri özellik değerlerini hesaplanan sonraki kez fiyat DayOpen arasındaki fark temel.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>StockTicker ve StockTickerHub sınıfları oluşturma

Sunucu istemciye etkileşim işlemek için SignalR hub'ı API kullanacaksınız. SignalR hub'ı sınıfından türetilen StockTickerHub sınıfı, bağlantıları ve yöntem çağrılarını istemcilerinden gelen işleyecek. Aynı zamanda hisse senedi verileri korumak ve istemci bağlantıları bağımsız olarak fiyat güncelleştirmeleri düzenli aralıklarla tetiklemek için bir zamanlayıcı nesnesini çalıştırmak gerekir. Hub örnekleri geçici olduğundan, bu işlevlerin bir Hub sınıfta yerleştirin. Bir Hub örneği bağlantılar ve istemciden sunucuya çağrılar gibi hub üzerindeki her bir işlemin oluşturulur. Bu nedenle, StockTicker ad ayrı bir sınıf içinde çalıştırmak hisse senedi verileri tutar, fiyatları güncelleştirir ve fiyat güncelleştirmeleri yayınlar mekanizması vardır.

![StockTicker yayın](tutorial-server-broadcast-with-signalr/_static/image5.png)

Bir başvuru her StockTickerHub örneğinden singleton StockTicker örneğine ayarlanmış gerekir böylece sunucuda çalıştırmak için StockTicker sınıfının bir örneği yalnızca istediğiniz. StockTicker sınıfı istemcilere stok verileri içeren ve güncelleştirmeleri tetikler ancak StockTicker Hub sınıfına değil çünkü yayın mümkün olması gerekir. Bu nedenle, StockTicker sınıfı SignalR hub'ı bağlantı bağlamı nesneye bir başvurusu almak zorundadır. Bunu ardından, SignalR bağlantısı bağlam nesnesi istemcilere yayınlamak için kullanabilirsiniz.

1. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve **Ekle | SignalR hub'ı sınıfı (v2)**.
2. Yeni hub'ı adı *StockTickerHub.cs*ve ardından **Ekle**. SignalR NuGet paketlerini projenize eklenir.
3. Şablon kodu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    [Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) sınıfı, istemcilerin sunucuda çağırabilir yöntemlerini tanımlamak için kullanılır. Bir yöntem tanımlama: `GetAllStocks()`. Bir istemci başlangıçta sunucuya bağlandığında, kendi geçerli fiyatlarla hisse tümünde listesini almak için bu yöntemi çağırır. Yöntemi, zaman uyumlu olarak yürütür ve dönüş `IEnumerable<Stock>` bellekten veri döndürmektir olduğundan. Yöntemin bir veritabanı araması veya bir web hizmeti çağrısı gibi bekleme içerir bir şey yaparak veri almak sahip belirtirsiniz `Task<IEnumerable<Stock>>` zaman uyumsuz işleme etkinleştirmek için dönüş değeri olarak. Daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - zaman uyumsuz olarak yürütülecek ne zaman](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

    İstemci üzerinde JavaScript kodunda Hub'ın nasıl başvurulacak HubName özniteliği belirtir. Bu öznitelik kullanmazsanız istemcideki varsayılan adı stockTickerHub bu durumda olacaktır sınıf adının başlamalıdır bir sürümüdür.

    StockTicker sınıfı oluşturduğunuzda, daha sonra anlatıldığı gibi sınıfı tek örneğini statik örneği özelliğinde oluşturulur. Kaç adet istemcinin bağlanın veya bağlantıyı kesin olsun bellekte StockTicker tek örneğini kalır ve bu örnek GetAllStocks yöntemi geçerli stok bilgileri döndürmek için kullandığı olduğundan.
4. Proje klasöründe yeni bir sınıf dosyası oluşturun, adlandırın *StockTicker.cs*ve ardından şablon kodu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    Birden çok iş parçacığı StockTicker kodu aynı örneği çalışacağı beri StockTicker sınıfı threadsafe olması gerekir.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Statik bir alana tek örnek depolama

    Statik kodu başlatır \_, sınıf ve bu örneği örneği özelliğiyle yedekler örneği alan olup, Oluşturucusu özel olarak işaretlendiğinden oluşturulabilir, sınıfı tek örneği. [Geç başlatma](https://msdn.microsoft.com/en-us/library/dd997286.aspx) için kullanılan \_değil ancak örnek oluşturma threadsafe olduğundan emin olmak üzere performansı artırmak için örnek alanı.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    Daha önce StockTickerHub sınıfında anlatıldığı gibi bir istemci sunucuya her bağlandığında StockTicker.Instance statik özelliğinden StockTicker tek örneği ayrı bir iş parçacığı çalıştıran StockTickerHub sınıfının yeni bir örneğini alır.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Bir ConcurrentDictionary stok veri depolama

    Oluşturucu başlatır \_istediğiniz hisse koleksiyonuyla bazı örnek stok verileri ve GetAllStocks istediğiniz hisse döndürür. Daha önce gördüğünüz gibi bu koleksiyonu hisse sırayla bir sunucu yöntemle istemcileri çağırabilirsiniz Hub sınıfında StockTickerHub.GetAllStocks tarafından döndürülür.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    İstediğiniz hisse koleksiyonu olarak tanımlanan bir [ConcurrentDictionary](https://msdn.microsoft.com/en-us/library/dd287191.aspx) iş parçacığı güvenliği için türü. Alternatif olarak, kullanabileceğinizi bir [sözlük](https://msdn.microsoft.com/en-us/library/xfhwa508.aspx) nesne ve değişiklik yaptığınız zaman sözlük açıkça kilitleyin.

    Bu örnek uygulama için bu Tamam bellekte uygulama verilerini depolamak için ve StockTicker örneği çıkarıldığından, veri kaybına olur. Gerçek bir uygulamada bir veritabanı gibi bir arka uç veri deposu ile çalışır.

    ### <a name="periodically-updating-stock-prices"></a>Stok fiyatları düzenli olarak güncelleştirme

    Düzenli aralıklarla rastgele temelinde hisse senedi fiyatları güncelleştirme yöntemlerini çağıran bir süreölçer nesnesi yukarı Oluşturucusu başlatır.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    UpdateStockPrices null durumu parametre olarak geçirir Zamanlayıcı tarafından çağrılır. Fiyatlar güncelleştirmeden önce bir kilit alınır \_updateStockPricesLock nesnesi. Kod, başka bir iş parçacığı zaten fiyatları güncelleştirme ve ardından listedeki her stoktaki TryUpdateStockPrice çağırır denetler. TryUpdateStockPrice yöntemi mi hisse senedi fiyatı değiştirmeye karar verir ve değiştirmek için ne kadar. Hisse senedi fiyatı değiştirdiyseniz BroadcastStockPrice bağlanan tüm istemciler için stok fiyat değişikliği yayını için çağrılır.

    \_UpdatingStockPrices bayrak olarak işaretli [volatile](https://msdn.microsoft.com/en-us/library/x13ttww7.aspx) erişimi threadsafe olduğundan emin olmak için.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    Gerçek bir uygulamada TryUpdateStockPrice yöntemi fiyatı aramak için bir web hizmeti çağırırdı; Bu kodda değişiklik rastgele bir rastgele sayı üreticisinin kullanır.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Böylece StockTicker sınıfı istemcilere yayınlayabilirsiniz SignalR bağlamı alma

    Fiyat değişikliklerinin burada StockTicker nesnesinde kaynaklı olduğundan, bu bağlanan tüm istemciler üzerinde bir updateStockPrice yöntemini çağırmak için gereken nesnesidir. Bir Hub sınıfı, bir API İstemci yöntemleri çağırmak için sahip, ancak StockTicker Hub sınıfından türemiyor ve herhangi bir Hub nesneye bir başvurusu yok. Bu nedenle, bağlı istemciler için yayın için SignalR bağlam örneği için StockTickerHub sınıfını almak ve istemcilerde yöntemlerini çağırmaya kullanmaya StockTicker sınıfı sahiptir.

    Singleton sınıf örneği, başvuru geçişleri oluşturucuya oluşturduğunda kodu SignalR bağlamı için bir başvuru alır ve isteğe bağlı olarak Oluşturucusu istemcileri özelliğinde yerleştirir.

    Bağlam yalnızca bir kez almak istediğiniz neden iki nedeni vardır: bağlamı alma pahalı bir işlem olduğundan ve bir kez alma sağlar istemcilere gönderilen iletilerin hedeflenen sırasını korunur.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    Bağlamının istemcileri özellik alma ve StockTickerClient özelliğinde koyma gibi görünen aynı Hub sınıfında istemci yöntemleri çağırmak için kod yazmanıza izin verir. Örneğin, tüm istemcilere yayınlamak için Clients.All.updateStockPrice(stock) yazabilirsiniz.

    İçinde BroadcastStockPrice aradığınız updateStockPrice yöntemi henüz yok; istemci üzerinde çalışan bir kod yazarken daha sonra eklersiniz. Clients.All çalışma zamanında ifadenin hesaplanacağı anlamına gelir dinamik olduğundan burada updateStockPrice başvurabilir. Bu yöntem çağrısı yürütüldüğünde, SignalR yöntem adı ve parametre değeri istemciye gönderir ve istemci updateStockPrice adlı bir yöntemi varsa, bu yöntem çağrılır ve parametre değeri geçirilir.

    Clients.All tüm istemcilere göndermek anlamına gelir. SignalR hangi istemcilerin veya göndermek için istemci gruplarının belirtmek için diğer seçenekler sunar. Daha fazla bilgi için bkz: [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>SignalR rota kaydetme

Sunucu kesecek ve SignalR için doğrudan hangi URL bilmek ister. Ekleyeceksiniz yapın ve OWIN başlangıç sınıfı.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | OWIN başlangıç sınıfı**. Sınıf adını **haline**.
2. Kodla **haline** aşağıdaki.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Sunucu kodu ayarlama tamamladınız. Sonraki bölümde istemci ayarlarsınız.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>İstemci kodu ayarlamanız

1. Proje klasöründe yeni bir HTML dosyası oluşturun ve adlandırın *StockTicker.html*.
2. Şablon kodu aşağıdaki kodla değiştirin.

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    HTML 5 sütunları, sütun başlığı ve tüm 5 sütunu kapsayan tek bir hücre ile bir veri satırı bir tablo oluşturur. Veri satırı "yükleniyor..." görüntüler ve yalnızca kısa bir süre içinde uygulama başladığında görüntülenir. JavaScript kodu satır kaldırın ve sunucudan alınan stok verilerle bunun yerine satır ekleyin.

    Komut dosyası etiketlerini jQuery komut dosyası, SignalR core komut dosyası, SignalR proxy'leri komut dosyası ve daha sonra oluşturacaksınız StockTicker komut dosyasını belirtin. "/ Signalr/hub" URL'yi belirtir, SignalR proxy'leri komut dosyası dinamik olarak oluşturulur ve Hub sınıfı yöntemleri için proxy yöntemleri için StockTickerHub.GetAllStocks bu durumda tanımlar. Tercih ederseniz, bu JavaScript dosyası el ile kullanarak oluşturabileceğiniz [SignalR yardımcı programları](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) ve MapHubs yöntem çağrısı içinde dinamik dosya oluşturma devre dışı bırakın.
3. > [!IMPORTANT]
 > JavaScript dosyası içinde başvurduğundan emin olun *StockTicker.html* doğrudur. Diğer bir deyişle, komut dosyası etiketinin (örnekte 1.10.2) jQuery sürümünü projenizin jQuery sürümü ile aynı olduğundan emin olun *betikleri* klasörünü ve komut dosyası etiketinin SignalR sürümünde SignalR ile aynı olduğundan emin olun projenizin sürümünde *betikleri* klasör. Komut dosyası etiketlerini dosya adlarında gerekiyorsa değiştirin.
4. İçinde **Çözüm Gezgini**, sağ *StockTicker.html*ve ardından **Başlangıç Sayfası Ayarla**.
5. Proje klasöründe yeni bir JavaScript dosyası oluşturun ve adlandırın *StockTicker.js*...
6. Şablon kodu aşağıdaki kodla değiştirin:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection SignalR proxy'leri başvuruyor. Kod StockTickerHub sınıfı için proxy için bir başvuru alır ve borsa takip değişkene koyar. Proxy adı [HubName] özniteliği tarafından ayarlanan adıdır:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    Tüm işlevleri ve değişkenler tanımlandıktan sonra dosyanın içindeki son satırının SignalR başlangıç işlevini çağırarak SignalR bağlantısı başlatır. Başlangıç işlevi zaman uyumsuz olarak yürütür ve döndürür bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/), anlamına gelir zaman uyumsuz işlemi tamamlandığında çağrılacak işlevin belirtmek için Bitti'yi işlevi çağırabilirsiniz...

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    İnit işlevi sunucuda getAllStocks işlevi çağırır ve sunucu stok tablosunu güncelleştirmek için bilgileri kullanır. Yöntem adı sunucuda pascal ortası olsa ortası istemci üzerinde büyük/küçük harfleri kullanmak zorunda varsayılan olarak, dikkat edin. Ortası büyük büyük/küçük harf kural yalnızca nesneleri yöntemleri için geçerlidir. Örneğin, stoka bakın. Simge ve hisse senedi. Fiyat veya değil, stock.symbol stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    İstemcide pascal büyük/küçük harf kullanmak istediğinizi veya tamamen farklı bir yöntem adını kullanmak istiyorsanız, HubMethodName özniteliği Hub yöntemiyle aynı şekilde tasarlamanız Hub sınıfının kendisi HubName özniteliği ile donatılmış.

    Init yöntemi stok nesne biçim özelliklerine göre arama formatStock sunucudan alınan stok her nesne için bir tablo satır için HTML oluşturulur ve göre çağırma supplant (üst kısmında tanımlanan *StockTicker.js*) ile stok nesne özellik değerlerini rowTemplate değişkeni yer tutucuları değiştirmek için. Sonuçta elde edilen HTML sonra stok tablosuna eklenir.

    Init içinde zaman uyumsuz başlangıç işlevi tamamlandıktan sonra çalıştırılan bir geri çağırma işlevini geçirerek çağırın. Başlangıç çağrıldıktan sonra ayrı bir JavaScript ifadesi olarak init adlı bağlantı kurarak tamamlamak başlangıç işlevi için beklenmeden hemen yürütülür işlevi başarısız. Bu durumda, sunucu bağlantı kurulmadan önce getAllStocks işlevi çağırmak init işlevi isteriz.

    Sunucu stoğu 's fiyat değiştiğinde bağlı istemcilerde updateStockPrice çağırır. İşlev çağrıları sunucudan kullanabilmesi için stockTicker proxy istemci özelliğine eklenir.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    İnit işlevi olduğu gibi bir tablo satıra sunucudan alınan bir stok nesnesi updateStockPrice işlevi biçimlendirir. Ancak, tabloya satır ekleme, yerine hisse senedi'nın geçerli satır tabloda bulur ve satır yeni bir bağlantıyla değiştirir.

<a id="test"></a>

## <a name="test-the-application"></a>Uygulamayı test etme

1. Uygulamayı hata ayıklama modunda çalıştırmak için F5 tuşuna basın.

    "... Satır, ardından ilk hisse senedi verileri görüntülenen kısa bir gecikmeyle yüklenirken" hisse senedi tablosu başlangıçta görüntüler ve değiştirmek hisse senedi fiyatları başlatın.

    ![Yükleme](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![İlk hisse senedi tablosu](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![Hisse senedi tablosu değişiklikleri sunucudan alınıyor](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. Tarayıcınızın adres çubuğundan URL'yi kopyalayın ve bir veya daha fazla yeni tarayıcı pencerelerini yapıştırın.

    İlk stok görünen ilk tarayıcı ile aynıdır ve aynı anda değişiklikleri gerçekleşir.
3. Tüm tarayıcılar kapatın ve yeni bir tarayıcı açın, sonra aynı URL'ye gidin.

    StockTicker tekil nesnesi hisse senedi tablosu görüntülenmesini istediğiniz hisse değiştirmeye devam etti gösterecek şekilde Server'da çalıştırmak devam etti. (Sıfır ilk tabloyla rakamları değiştirmek görmüyorum.)
4. Tarayıcıyı kapatın.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Günlük kaydını etkinleştir

SignalR giderilmesine yardımcı olmak için istemcide etkinleştirebilirsiniz bir yerleşik günlük işlevi vardır. Bu bölümdeki günlüğe yazılmasını etkinleştirmek ve nasıl günlükleri SignalR kullanarak, aşağıdaki taşıma yöntemleri size Göster örneklere bakın:

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), IIS 8 ve geçerli tarayıcılar tarafından desteklenir.
- [Sunucu tarafından gönderilen olaylar](http://en.wikipedia.org/wiki/Server-sent_events), Internet Explorer dışında tarayıcılar tarafından desteklenir.
- [Devamlı çerçeve](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), Internet Explorer tarafından desteklenmiyor.
- [AJAX yoklama uzun](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), tüm tarayıcılar tarafından desteklenir.

Verilen her bağlantı için SignalR hem sunucu hem de istemci destek en iyi aktarım yöntemi seçer.

1. Açık *StockTicker.js* ve bir dosyanın sonunda bağlantı başlatır kodundan hemen önce günlüğe kaydetmeyi etkinleştirmek için kod satırı ekleyin:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. Projeyi çalıştırmak için F5 tuşuna basın.
3. Tarayıcınızın geliştirici araçları penceresini açın ve konsol günlükleri görmek için seçin. Signalr aktarım yöntemi için yeni bir bağlantı anlaşması günlükleri görmek için sayfayı yenileyin gerekebilir.

    Windows 8 (IIS 8) Internet Explorer 10 çalıştırıyorsanız, aktarım WebSockets yöntemidir.

    ![IE 10 IIS 8 Konsol](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Internet Explorer 10 Windows 7 (IIS 7.5) çalıştırıyorsanız, aktarım IFRAME yöntemidir.

    ![IE 10 konsolu, IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    Firefox'ta, Firebug bir konsol penceresi almak için eklentisini yükleyin. Windows 8 (IIS 8) Firefox 19 çalıştırıyorsanız, aktarım WebSockets yöntemidir.

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Windows 7 (IIS 7.5) Firefox 19 çalıştırıyorsanız, aktarım sunucu tarafından gönderilen olaylar yöntemidir.

    ![IIS 7.5 Firefox 19 konsol](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Yükleme ve tam StockTicker örnek gözden geçirin

Tarafından yüklenen StockTicker uygulama [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet paketi sıfırdan az önce oluşturduğunuz Basitleştirilmiş sürümden daha fazla özellik içerir. Öğreticinin bu bölümünde NuGet paketini yükleyin ve yeni özellikler ve bunları uygulayan kodunu gözden geçirin. Bu öğreticinin önceki adımları gerçekleştirmeden paketi yüklerseniz OWIN başlangıç sınıfı projenize eklemeniz gerekir. Bu adım için NuGet paketi readme.txt dosyasında açıklanmıştır.

### <a name="install-the-signalrsample-nuget-package"></a>SignalR.Sample NuGet paketini yükleyin

1. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.
2. İçinde **NuGet paketlerini Yönet** iletişim kutusu, tıklatın **çevrimiçi**, girin *SignalR.Sample* içinde **arama çevrimiçi** kutusuna ve ardından tıklatın **Yükleme** içinde **SignalR.Sample** paket.

    ![SignalR.Sample paketini yükle](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. İçinde **Çözüm Gezgini**, genişletin *SignalR.Sample* SignalR.Sample paketi yükleyerek oluşturulduğu klasör.
4. İçinde *SignalR.Sample* klasörünü sağ tıklatın *StockTicker.html*ve ardından **başlangıç sayfası olarak ayarla**.

    > [!NOTE]
    > Yükleme için SignalR.Sample NuGet paketi uygulamanızdaki jQuery sürümü değişebilir, *betikleri* klasör. Yeni *StockTicker.html* paketi yükler dosya *SignalR.Sample* klasörü orijinal çalıştırmakisteyipistemediğiniziancakyükleyenpaket,jQuerysürümüyleeşitlemeolacaktır*StockTicker.html* dosyasını yeniden, komut dosyası etiketinin jQuery başvurusunda önce güncelleştirmeniz gerekebilir.

### <a name="run-the-application"></a>Uygulamayı çalıştırın

1. Uygulamayı çalıştırmak için F5 tuşuna basın.

    Daha önce gördüğünüzle kılavuza ek olarak, tam borsa uygulama aynı stok verileri görüntüleyen bir yatay kaydırma penceresi gösterilir. Uygulamayı ilk kez çalıştırdığınızda "pazar" "kapalı" ve statik bir tablo ve kaydırma değil bir borsa takip pencere görürsünüz.

    ![StockTicker ekran Başlat](tutorial-server-broadcast-with-signalr/_static/image14.png)

    Tıkladığınızda **açık Pazar**, **dinamik stok borsa takip** kutusunu yatay kaydırma, ve sunucu stok fiyat değişiklikleri rasgele olarak düzenli aralıklarla yayınlanacak başlayan. Hisse senedi fiyatı her zaman değişikliği, her ikisi de **dinamik stok tablo** kılavuz ve **dinamik stok borsa takip** kutusunu güncelleştirilir. Hisse 's fiyat değişikliği pozitif olduğunda hisse senedi yeşil bir arka plan gösterilir ve değişiklik negatif olduğunda hisse senedi kırmızı bir arka plan ile gösterilir.

    ![Pazar StockTicker uygulamasını açın](tutorial-server-broadcast-with-signalr/_static/image15.png)

    **Kapat Pazar** düğmesi değişiklikleri ve kaydırma borsa takip durur ve **sıfırlama** düğme fiyat değişiklikleri başlatmadan önce bu tüm hisse senedi verileri ilk durumuna sıfırlar. Daha fazla tarayıcı pencerelerini açın ve aynı URL'ye gidin, aynı zamanda her tarayıcıda dinamik olarak güncelleştirilen aynı veri bakın. Düğmeleri birine tıkladığınızda, tüm tarayıcılar aynı anda aynı şekilde yanıt verir.

### <a name="live-stock-ticker-display"></a>Dinamik stok borsa takip görüntüleme

**Dinamik stok borsa takip** sırasız bir listesini tek bir satıra CSS stilleri göre biçimlendirilmiş bir div öğesinin içinde görüntülenir. Sayaç başlatılır ve tablo aynı şekilde güncelleştirildi: yer tutucuları değiştirerek bir &lt;li&gt; şablonu dizesi ve dinamik olarak ekleme &lt;li&gt; öğelerine &lt;ul&gt; öğesi. Kaydırma DIV içinde sırasız liste sol kenar boşluğu değiştirmek için jQuery animasyon olarak oluşturmak işlevini kullanarak gerçekleştirilir

Bandı HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

Bandı CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

Kolaylaştırır jQuery kodu kaydırma:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>İstemci çağırabilirsiniz sunucuda ek yöntemleri

StockTickerHub sınıfı, istemci çağırabilirsiniz dört ek yöntemleri tanımlar:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

Sayfanın üstündeki düğmelere yanıt OpenMarket, CloseMarket ve sıfırlama çağrılır. Bunlar, tüm istemcilere hemen yayılır durumundaki bir değişikliği tetikleyen bir istemci desenini gösterir. Bu yöntemlerin her biri bir yöntem StockTicker sınıfında Pazar durumu değiştirin ve ardından yeni bir durum yayınlar bu etkileri çağırır.

StockTicker sınıfında Pazar durumunu MarketState enum değeri döndüren bir MarketState özelliği tarafından korunur:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Pazar durum değişikliği yöntemlerin her biri bunu kilit bloğu içinde StockTicker sınıfı threadsafe olduğu:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Bu kod threadsafe, olduğundan emin olmak için \_MarketState özelliği yedekler marketState alanı geçici işaretli

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

İstemcide tanımlanan farklı yöntemleri çağırmak dışında BroadcastMarketStateChange ve BroadcastMarketReset yöntemleri zaten gördüğünüz, BroadcastStockPrice yöntemi benzerdir:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Sunucu çağırabilirsiniz istemci üzerinde ek işlevleri

UpdateStockPrice işlevi artık kılavuz ve borsa takip görüntü işleme ve kırmızı ve yeşil renkleri Flash jQuery.Color kullanır.

Yeni işlevler *SignalR.StockTicker.js* etkinleştirme ve devre dışı bırak düğmeleri temel alarak pazara durumu ve bunlar başlatmak veya durdurmak borsa takip penceresini yatay kaydırma. Birden çok işlevleri ticker.client için eklenmekte olan bu yana [jQuery genişletmek işlevi](http://api.jquery.com/jQuery.extend/) bunları eklemek için kullanılır.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Bağlantı kurulduktan sonra ek istemci kurulumu

İstemci bağlantı kurar sonra yapmak için bazı ek iş vardır: Pazar açık veya kapalı uygun marketOpened veya marketClosed işlevini çağırın ve sunucu yöntem çağrılarını düğmelere eklemek için olup olmadığını öğrenin.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Bağlantı kurulduktan sonra böylece kodu kullanılabilir önce sunucu yöntemlerini çağırmaya deneyemez server yöntemlerini kadar düğmeleri kadar kablolu arabirimlerdir değil.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide iletileri sunucudan bağlanan tüm istemciler, düzenli aralıklarla hem bildirimlere yanıt olarak herhangi bir istemciden yayınlar SignalR uygulama programı nasıl öğrendiniz. Sunucu durumunu korumak üzere birden çok iş parçacıklı tek bir örneği kullanılarak desenini de çok oyunculu oyun çevrimiçi senaryolarda da kullanılabilir. Bir örnek için bkz: [SignalR tabanlı ShootR oyun](https://github.com/NTaylorMullen/ShootR).

Eşler arası iletişim senaryoları Göster öğreticileri için bkz: [SignalR ile çalışmaya başlama](introduction-to-signalr.md) ve [SignalR ile gerçek zamanlı güncelleştirme](tutorial-high-frequency-realtime-with-signalr.md).

Daha gelişmiş SignalR geliştirme kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [ASP.NET SignalR](../../index.md)
- [SignalR projesi](http://signalr.net/)
- [SignalR Github ve örnekler](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Bir SignalR uygulamayı Azure'a dağıtma hakkında bir kılavuz için bkz: [SignalR kullanarak Azure App Service'te Web uygulamalarını](../deployment/using-signalr-with-azure-web-sites.md). Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında ayrıntılı bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).
