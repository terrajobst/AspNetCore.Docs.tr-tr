---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Öğretici: SignalR 2 ile sunucu yayını | Microsoft Docs'
author: tdykstra
description: Bu öğreticide, ASP.NET SignalR 2 sunucu yayın işlevselliği sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir.
ms.author: riande
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a6014e604613492db91b2dc6f846c3c73d938d99
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099305"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Öğretici: SignalR 2 ile yayın sunucusu

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Bu öğreticide, ASP.NET SignalR 2 sunucu yayın işlevselliği sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir. Sunucu yayın sunucuyu istemcilere iletişimler başlatır anlamına gelir.

Bu öğreticide oluşturacağınız uygulama bir bandı yayın sunucusu işlevselliği için tipik bir senaryo benzetimini yapar. Düzenli olarak, sunucu rastgele hisse senedi fiyatlarına güncelleştirir ve bağlanan tüm istemciler için güncelleştirmeleri yayınlayın. Tarayıcı, sayılar ve semboller **değiştirme** ve **%** sütunları dinamik olarak değiştirme sunucudan yanıt bildirimleri. Aynı URL'ye ek tarayıcılar açarsanız, bunların tümü aynı verilere ve verilere aynı değişiklikleri aynı anda gösterin.

![Web oluşturma](tutorial-server-broadcast-with-signalr/_static/image1.png)

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Projeyi oluşturma
> * Sunucu kodunu ayarlayın
> * Sunucu kodu inceleyin
> * İstemci kodunu ayarlayın
> * İstemci kodu inceleyin
> * Uygulamayı test etme
> * Günlü kaydını etkinleştir

> [!IMPORTANT]
> Uygulama oluşturma adımlarında size çalışmak istemiyorsanız, yeni bir boş bir ASP.NET Web uygulaması projesinde SignalR.Sample paketini yükleyebilirsiniz. Bu öğreticideki adımları gerçekleştirmeden NuGet paketi yüklerseniz, yönergeleri izlemelidir *readme.txt* dosya. Bir OWIN başlangıç eklemeniz paketini çalıştırmak için hangi çağrıları sınıfı `ConfigureSignalR` yüklenen paketteki yöntemi. OWIN başlangıç sınıfı eklemezseniz bir hata alırsınız. Bkz: [StockTicker örneği yüklemek](#install-the-stockticker-sample) bu makalenin.


## <a name="prerequisites"></a>Önkoşullar

 * [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü.

## <a name="create-the-project"></a>Projeyi oluşturma

Bu bölümde, boş bir ASP.NET Web uygulaması oluşturmak için Visual Studio 2017'yi kullanmayı gösterir.

1. Visual Studio kullanarak ASP.NET Web uygulaması oluşturun.

    ![Web oluşturma](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. İçinde **yeni ASP.NET Web uygulaması - SignalR.StockTicker** penceresinde bırakın **boş** seçin ve seçilen **Tamam**.

## <a name="set-up-the-server-code"></a>Sunucu kodunu ayarlayın

Bu bölümde, sunucu üzerinde çalışan kodu ayarlayın.

### <a name="create-the-stock-class"></a>Stok sınıfı oluşturma

Oluşturarak başlayın *hisse senedi* model depolamak ve stok hakkında bilgi iletmek için kullanacağınız sınıfı.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **sınıfı**.

1. Sınıf adı *hisse senedi* ve projeye ekleyin.

1. Değiştirin *Stock.cs* Bu kod dosyası:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Hisse oluştururken ayarladığınız iki özellik `Symbol` (örneğin, Microsoft MSFT) ve `Price`. Diğer özellikleri nasıl ve ne zaman ayarlamak bağımlı `Price`. İlk kez ayarladığınız `Price`, yayıldığı `DayOpen`. Bundan sonra ayarladığınızda `Price`, uygulamayı hesaplar `Change` ve `PercentChange` arasındaki fark temel özellik değerlerini `Price` ve `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>StockTickerHub ve StockTicker sınıfları oluşturma

SignalR hub'ı API sunucusu istemci etkileşimi işlemek için kullanacaksınız. A `StockTickerHub` türetilen sınıf `SignalRHub` sınıfı, bağlantı ve yöntem çağrıları istemcilerinden gelen işleyecek. Aynı zamanda Stok verileri korumak ve çalıştırmak gereken bir `Timer` nesne. `Timer` Nesne düzenli aralıklarla fiyat güncelleştirmeleri istemci bağlantıları bağımsız tetikleyin. Bu işlevler konulamıyor bir `Hub` çünkü Hubs'a geçicidir. Uygulamayı oluşturur bir `Hub` bağlantıları ve istemciden sunucuya çağrılar gibi hub'ında her görev için sınıf örneği. Bu nedenle ayrı bir sınıf içinde çalıştırmak stok verileri tutar, fiyatları güncelleştirir ve fiyat güncelleştirmeleri yayınlar mekanizması vardır. Sınıf adı `StockTicker`.

![Yayın StockTicker gelen](tutorial-server-broadcast-with-signalr/_static/image3.png)

Yalnızca bir örneğini istediğiniz `StockTicker` sınıfı, bu nedenle her bir başvuru ayarlamanız gerekir sunucuda çalıştırılacak `StockTickerHub` tekli örneğine `StockTicker` örneği. `StockTicker` Sınıfında stok verileri içeren ve güncelleştirmeleri Tetikleyiciler olduğundan istemcilere yayın ancak `StockTicker` değil bir `Hub` sınıfı. `StockTicker` Sınıfında SignalR hub'ı bağlantı kapsamı nesnesine bir başvuru almak. Bunu daha sonra SignalR bağlantı bağlam nesnesi istemcilere yayınlamak için kullanabilirsiniz.

#### <a name="create-stocktickerhubcs"></a>StockTickerHub.cs oluşturma

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.

1. İçinde **Yeni Öğe Ekle - SignalR.StockTicker**seçin **yüklü** > **Visual C#**   >  **Web**  >  **SignalR** seçip **SignalR Hub sınıfı (v2)**.

1. Sınıf adı *StockTickerHub* ve projeye ekleyin.

    Bu adımda oluşturulur *StockTickerHub.cs* sınıf dosyası. Eşzamanlı olarak projeye SignalR destekleyen bir dizi komut dosyaları ve derleme başvurularını ekler.

1. Değiştirin *StockTickerHub.cs* Bu kod dosyası:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Dosyayı kaydedin.

Uygulamanın kullandığı [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) istemcilerin sunucuda çağırabilir yöntemleri tanımlamak için sınıf. Bir yöntem tanımlayacağınız: `GetAllStocks()`. Bir istemci sunucuya ilk kez bağlandığında, geçerli fiyatları ile stocks tümünün listesini almak için bu yöntemi çağırır. Yöntem zaman uyumlu olarak çalıştırabilir ve dönüş `IEnumerable<Stock>` nedeniyle bellekten veri döndürüyor.

Yöntem bir veritabanı araması veya bir web hizmeti çağrısı gibi bir bekleme kapsayacaktır bir şey yaparak verileri almak sahip olduğu belirtirsiniz `Task<IEnumerable<Stock>>` zaman uyumsuz işleme etkinleştirmek için dönüş değeri. Daha fazla bilgi için [ASP.NET SignalR Hubs API Kılavuzu - sunucu - zaman zaman uyumsuz olarak yürütülecek](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

`HubName` Özniteliği, uygulama istemcide JavaScript kod hub'ı nasıl Bakacağınız belirtir. Bu öznitelik kullanmazsanız istemcideki varsayılan adı camelCase bu durumda sınıf adı sürümüdür `stockTickerHub`.

Daha sonra göreceğiniz üzere oluşturduğunuzda `StockTicker` sınıfı uygulaması oluşturur, sınıf tekil örneğini, statik `Instance` özelliği. Tekil örneğini `StockTicker` kaç adet istemcinin bağlanın veya bağlantıyı kesin ne olursa olsun bellek içindedir. Bu örneği nedir `GetAllStocks()` yöntemi geçerli hisse senedi bilgi almak için kullanır.

#### <a name="create-stocktickercs"></a>StockTicker.cs oluşturma

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **sınıfı**.

1. Sınıf adı *StockTicker* ve projeye ekleyin.

1. Değiştirin *StockTicker.cs* Bu kod dosyası:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Tüm iş parçacıklarının StockTicker kod'ın aynı örneğinin çalışacağı beri StockTicker sınıfı iş parçacığı açısından güvenli olması gerekir.

### <a name="examine-the-server-code"></a>Sunucu kodu inceleyin

Sunucu kodu inceleyin, uygulamanın nasıl çalıştığını anlamanıza yardımcı olur.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Statik bir alana tekil örneğini depolama

Statik kod başlatır `_instance` yedekler alan `Instance` özelliğiyle sınıfının örneği. Oluşturucusu özel olduğundan, uygulama oluşturabileceğiniz sınıfın yalnızca örneğidir. Uygulamanın kullandığı [yavaş başlatma](/dotnet/framework/performance/lazy-initialization) için `_instance` alan. Performansla ilgili nedenlerden dolayı değil. Bu örnek oluşturma iş parçacığı açısından güvenli olduğundan emin olmaktır.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Bir istemci sunucuya her bağlandığında StockTicker tekil örnek içinde ayrı bir iş parçacığı çalıştırırken StockTickerHub sınıfının yeni bir örneğini alır `StockTicker.Instance` statik özelliği gördüğünüz daha önce `StockTickerHub` sınıfı.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Bir ConcurrentDictionary stok verileri depolama

Oluşturucu başlatır `_stocks` bazı örnek verilerle stok, koleksiyon ve `GetAllStocks` hisse senetleri döner. Daha önce bahsettiğim gibi Bu hisse topluluğu tarafından döndürülen `StockTickerHub.GetAllStocks`, bir sunucu yöntemde olduğu `Hub` istemcileri çağırabilirsiniz sınıfı.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

Hisse koleksiyon olarak tanımlanan bir [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) türü için iş parçacığı güvenliği. Alternatif olarak, kullanabileceğinizi bir [sözlük](https://msdn.microsoft.com/library/xfhwa508.aspx) nesne ve ona değişiklikler yaptığınızda sözlük açıkça kilitleme.

Bu örnek uygulama için Tamam bellekte uygulama verilerini depolamak ve uygulama, siler, veri kaybına neden olduğu `StockTicker` örneği. Gerçek bir uygulamada bir veritabanı gibi bir arka uç veri deposu ile işe yarar.

#### <a name="periodically-updating-stock-prices"></a>Hisse senedi fiyatlarına düzenli aralıklarla güncelleştiriliyor

Oluşturucu başlatılırken bir `Timer` nesnesini düzenli aralıklarla hisse senedi fiyatlarına rastgele olarak güncelleştiren yöntemleri çağırır.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` çağrıları `UpdateStockPrices`, hangi state parametresi null geçirir. Uygulama kilidi vereceğine fiyatları güncelleştirmeden önce `_updateStockPricesLock` nesne. Başka bir iş parçacığı zaten fiyatları güncelleştiriyor ve onu çağıran kodu denetler `TryUpdateStockPrice` listedeki her stoktaki. `TryUpdateStockPrice` Yöntemi hisse senedi fiyatının değiştirip değiştirmemeye karar verir ve değiştirmek için ne kadar. Hisse senedi fiyatının değişirse, uygulamayı çağırır `BroadcastStockPrice` bağlı istemcileri hisse senedi fiyatı değişiklik tüm yayın.

`_updatingStockPrices` Atanan bayrağı [geçici](https://msdn.microsoft.com/library/x13ttww7.aspx) iş parçacığı açısından güvenli olduğundan emin olmak için.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

Gerçek bir uygulamada `TryUpdateStockPrice` yöntemi fiyatı aramak için bir web hizmeti çağrısı. Bu kodda, uygulama değişiklik rastgele bir rasgele sayı üreteci kullanır.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Böylece StockTicker sınıfı istemcilere yayınlayabilirsiniz SignalR bağlamı alma

Fiyat değişiklikleri de burada kaynaklı olduğundan `StockTicker` nesnesini çağırmak için gereken nesne olduğu bir `updateStockPrice` bağlanan tüm istemciler yöntemi. İçinde bir `Hub` sınıf, sahip olduğunuz bir API İstemci yöntemleri çağırmak için ancak `StockTicker` öğesinden türetilen değil `Hub` sınıfı ve herhangi bir başvuru yoktur `Hub` nesne. Bağlı istemciler için yayın `StockTicker` sınıfında SignalR bağlam örneğinin `StockTickerHub` sınıfı ve bu istemcilerde yöntemleri çağırmak için kullanın.

Başvuran geçişleri oluşturucuya tekil sınıfı örneğini oluşturduğunda, kod SignalR bağlamı için bir başvuru alır ve oluşturucu koyar `Clients` özelliği.

Yalnızca bir kez bağlamı almak istediğiniz neden iki nedeni vardır: bağlamı alma, pahalı bir görev ve yapar emin uygulamayı hedeflenen istemcilere gönderilen iletilerin sırasını korur. sonra alınıyor.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Başlama `Clients` bağlamı ve yerleştirme özellik `StockTickerClient` özelliği, içinde olduğu gibi aynı görünür istemci yöntemleri çağırmak için kod yazmanıza olanak sağlar bir `Hub` sınıfı. Örneğin, tüm istemcilere, yayın yazabilirsiniz `Clients.All.updateStockPrice(stock)`.

`updateStockPrice` Numarasını arıyoruz yöntemi `BroadcastStockPrice` henüz mevcut değil. İstemcide çalışan kod yazdığınızda, daha sonra ekleyeceksiniz. Başvurabilirsiniz `updateStockPrice` burada çünkü `Clients.All` uygulama çalışma zamanında ifade değerlendirme yani dinamik olduğundan. Bu yöntem çağrısının yürütüldüğünde, SignalR yöntem adı ve parametre değeri istemciye gönderir ve istemci adlı yöntemi varsa `updateStockPrice`, uygulama bu yöntemi çağırın ve parametre değerini geçirin.

`Clients.All` tüm istemcilere Gönder anlamına gelir. SignalR, hangi istemcilerin veya göndermek için istemci gruplarının belirtmek için diğer seçenekler sunar. Daha fazla bilgi için [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>SignalR yol kaydetme

Sunucunun kesecek ve SignalR ile doğrudan hangi URL'sini de bilmeniz gerekir. Bunu yapmak için OWIN başlangıç sınıfı ekleyin:

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.

1. İçinde **Yeni Öğe Ekle - SignalR.StockTicker** seçin **yüklü** > **Visual C#**   >  **Web** ve ardından **OWIN başlangıç sınıfı**.

1. Sınıf adı *başlangıç* seçip **Tamam**.

1. Varsayılan kodda değiştirin *Startup.cs* Bu kod dosyası:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Artık sunucu kodu ayarlama tamamladınız. Sonraki bölümde, istemci ayarlarsınız.

## <a name="set-up-the-client-code"></a>İstemci kodunu ayarlayın

Bu bölümde, istemcide çalışan kodu ayarlayın.

### <a name="create-the-html-page-and-javascript-file"></a>JavaScript dosyası ve HTML sayfası oluşturma

HTML sayfasını verileri görüntüler ve JavaScript dosyasını verileri düzenler.

#### <a name="create-stocktickerhtml"></a>StockTicker.html oluşturma

İlk olarak, HTML istemcisi ekleyeceksiniz.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **HTML sayfası**.

1. Dosya adı *StockTicker* seçip **Tamam**.

1. Varsayılan kodda değiştirin *StockTicker.html* Bu kod dosyası:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    HTML beş sütun, bir üst bilgi satırı ve tüm beş sütun kapsayan tek bir hücre içeren bir veri satırı bir tablo oluşturur. Veri satırı, "kısa bir süre içinde uygulama başlatıldığında yükleniyor..." gösterir. JavaScript kodu satır kaldırın ve sunucudan alınan stok verilerini kendi yerde satır ekleyin.

    Komut dosyası etiketlerini belirtin:

    * JQuery betik dosyası.

    * SignalR çekirdek betik dosyası.

    * SignalR proxy'leri betik dosyası.

    * Daha sonra oluşturacağınız StockTicker betik dosyası.

    Uygulama, dinamik olarak SignalR proxy'leri betik dosyası oluşturur. "/ Signalr/hubs" URL'sini belirtir ve proxy için Hub sınıfına, bu durumda, üzerinde yöntemleri için yöntemlerini `StockTickerHub.GetAllStocks`. Tercih ederseniz, bu JavaScript dosyasını el ile kullanarak oluşturabileceğiniz [SignalR yardımcı programları](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). Dinamik dosya oluşturma, devre dışı bırakmak unutmayın `MapHubs` yöntem çağrısı.

1. İçinde **Çözüm Gezgini**, genişletme **betikleri**.

    JQuery ve SignalR için betik kitaplıkları projesinde görünür.

    > [!IMPORTANT]
    > Paket Yöneticisi SignalR betikleri daha sonraki bir sürümünü yükler.

1. Komut dosyalarını projedeki sürümleri değerine karşılık gelen kod bloğundaki komut dosyası başvuruları güncelleştirin.

1. İçinde **Çözüm Gezgini**, sağ *StockTicker.html*ve ardından **Başlangıç Sayfası Ayarla**.

#### <a name="create-stocktickerjs"></a>StockTicker.js oluşturma

Artık JavaScript dosyası oluşturun.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **JavaScript dosyası**.

1. Dosya adı *StockTicker* seçip **Tamam**.

1. Bu kodu ekleyin *StockTicker.js* dosyası:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>İstemci kodu inceleyin

İstemci kodu inceleyin, istemci kodu, iş uygulaması yapmak için sunucu kodu ile nasıl etkileşime gireceğini öğrenmenize yardımcı olur.

#### <a name="starting-the-connection"></a>Bağlantı başlatılıyor

`$.connection` SignalR proxy'leri için ifade eder. Kod proxy için bir başvuru edinir `StockTickerHub` sınıfı ve bunu koyar `ticker` değişkeni. Ara sunucu adını ayarlayın tarafından addır `HubName` özniteliği:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Tüm değişkenlerin ve işlevlerin tanımladıktan sonra dosyayı kodda son satırının SignalR bağlantı SignalR çağırarak başlatır `start` işlevi. `start` İşlev zaman uyumsuz olarak yürütür ve döndürür bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/). Uygulama, zaman uyumsuz eylem tamamlandığında çağırılacak işlev belirtmek için yapılan işlevi çağırabilir.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Tüm stocks alma

`init` İşlev çağrılarında `getAllStocks` işlevi sunucuya ve sunucu Stok tablosu bilgileri kullanır. Varsayılan olarak, yöntem adı sunucu üzerinde pascal harfleri olsa bile camelCasing istemcide kullanmak zorunda olduğuna dikkat edin. CamelCasing kural yalnızca nesneleri yöntemleri için geçerlidir. Örneğin, başvurmanız `stock.Symbol` ve `stock.Price`değil `stock.symbol` veya `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

İçinde `init` yöntemi uygulaması oluşturur HTML çağırarak sunucudan alınan stok her nesne için bir tablo satır için `formatStock` biçimi özelliklerine `stock` nesnesi ve ardından çağırarak `supplant` içindekiyertutucularıdeğiştirmekiçin`rowTemplate` değişken ile `stock` nesnesi özellik değerleri. Sonuçta elde edilen HTML sonra stok tablosuna eklenir.

> [!NOTE]
> Çağırmanızı `init` bunu olarak geçirerek bir `callback` sonra zaman uyumsuz yürütülen işlevi `start` işlev biter. Aradığınız varsa `init` arama sonra ayrı bir JavaScript ifadesi olarak `start`, bağlantı kurma tamamlanması için başlangıç işlevi beklenmeden hemen çalışır çünkü işlev başarısız olur. Bu durumda, `init` işlevi çağırmak deneyin `getAllStocks` uygulama sunucu bağlantısı kurmadan önce işlev.

#### <a name="getting-updated-stock-prices"></a>Güncelleştirilmiş hisse senedi fiyatlarına alma

Sunucu hisse ait fiyat değiştiğinde çağırdığı `updateStockPrice` bağlı istemciler üzerinde. Uygulama istemci özelliğine işlevi ekler `stockTicker` çağrıları sunucudan kullanabilmesi için proxy.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

`updateStockPrice` Sunucudan alınan bir tabloya bir stok satır olarak aynı şekilde işlev biçimleri `init` işlevi. Tabloya satır ekleme, yerine bu tabloda hisse senedi'nın geçerli satırı bulur ve satır yenisiyle değiştirir.

## <a name="test-the-application"></a>Uygulamayı test etme

Emin olmak için uygulamayı test edebilirsiniz çalışmaktadır. Hisse senedi fiyatlarına geciktirmeye ile canlı Stok tablosu görüntülemek tüm tarayıcı pencerelerini görürsünüz.

1. Araç çubuğunda, açma **betik hata ayıklamasını** ve ardından uygulamayı hata ayıklama modunda çalıştırmak için Yürüt düğmesini seçin.

    ![Hata ayıklama modu ve Çalıştır'ı seçerek kullanıcı görüntüsü.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Görüntüleyen bir tarayıcı penceresi açılacaktır **hisse senedi tablo canlı**. Stok tablosu başlangıçta "yükleniyor..." satır, daha sonra kısa bir süre sonra uygulamayı ilk stok verileri gösterir ve değiştirmek hisse senedi fiyatlarına başlatın gösterir.

1. Tarayıcıdan URL'yi kopyalayın, diğer iki tarayıcılar açın ve URL'leri adresi çubukları yapıştırın.

    İlk stok görünen ilk tarayıcı ile aynıdır ve değişiklikler aynı anda gerçekleşir.

1. Tüm tarayıcıları kapatın, yeni bir tarayıcı açın ve aynı URL'ye gidin.

    Sunucuda çalıştırılacak StockTicker tekil nesnesi devam. **Hisse senedi tablo canlı** stocks değiştirmeye devam gösterir. Rakamları değiştirmek tablonun ilk sıfır görmüyorum.

1. Tarayıcıyı kapatın.

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

SignalR istemcide giderilmesine yardımcı olmak için etkinleştirebileceğiniz bir yerleşik günlük işleve sahiptir. Bu bölümde, günlüğü etkinleştirmek ve nasıl günlükleri SignalR kullanarak, aşağıdaki taşıma yöntemleri size gösteren örneklere bakın:

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), IIS 8 ve geçerli tarayıcılar tarafından desteklenir.

* [Sunucu tarafından gönderilen olayları](http://en.wikipedia.org/wiki/Server-sent_events), Internet Explorer dışındaki tarayıcıları tarafından desteklenmiyor.

* [Sonsuza kadar çerçeve](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), Internet Explorer tarafından desteklenmiyor.

* [AJAX uzun yoklama](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), tüm tarayıcılar tarafından desteklenir.

Verilen herhangi bir bağlantısı için hem sunucu hem de istemci destekleyen en iyi bir aktarım yöntemi SignalR seçer.

1. Açık *StockTicker.js*.

1. Vurgulanan bu dosyanın sonuna bağlantı başlatır kodundan hemen önce günlüğe kaydetmeyi etkinleştirmek için kod satırını ekleyin:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Tuşuna **F5** projeyi çalıştırın.

1. Tarayıcınızın geliştirici araçları penceresini açın ve konsol günlükleri görmek için seçin. SignalR aktarım yöntemi için yeni bir bağlantı anlaşması günlükleri görmek için sayfayı yenilemeniz gerekebilir.

    * Internet Explorer 10, Windows 8'de (IIS 8) çalıştırıyorsanız, aktarım yöntemdir **WebSockets**.

    * Internet Explorer 10, Windows 7'de (IIS 7.5) çalıştırıyorsanız, aktarım yöntemdir **iframe**.

    * Windows 8 (IIS 8)'de Firefox 19 çalıştırıyorsanız, aktarım yöntemdir **WebSockets**.

        > [!TIP]
        > ' De Firefox, Firebug bir konsol penceresi almak için eklentisini yükleyin.

    * Windows 7 (IIS 7.5)'de Firefox 19 çalıştırıyorsanız, aktarım yöntemdir **sunucu tarafından gönderilen** olayları.

## <a name="install-the-stockticker-sample"></a>StockTicker örnek yükleyin

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) StockTicker uygulamayı yükler. NuGet paketini sıfırdan oluşturduğunuz Basitleştirilmiş sürümden daha fazla özellik içerir. Öğreticinin bu bölümünde NuGet paketini yükleyin ve yeni özellikleri ve bunları uygulayan kod gözden geçirin.

> [!IMPORTANT]
> Bu öğreticinin önceki adımları gerçekleştirmeden paketi yüklerseniz, OWIN başlangıç sınıfı projenize eklemeniz gerekir. Bu NuGet paketi için readme.txt dosyasını, bu adım açıklanır.

### <a name="install-the-signalrsample-nuget-package"></a>SignalR.Sample NuGet paketini yükle

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **NuGet paketlerini Yönet**.

1. İçinde **NuGet Paket Yöneticisi: SignalR.StockTicker**seçin **Gözat**.

1. Gelen **paket kaynağı**seçin **nuget.org**.

1. Girin *SignalR.Sample* seçip arama kutusuna **Microsoft.AspNet.SignalR.Sample** > **yükleme**.

1. İçinde **Çözüm Gezgini**, genişletme *SignalR.Sample* klasör.

    SignalR.Sample paket yükleme klasörü ve içeriği oluşturulur.

1. İçinde *SignalR.Sample* klasörü sağ tıklatın *StockTicker.html*ve ardından **başlangıç sayfası olarak ayarla**.

    > [!NOTE]
    > Yükleme SignalR.Sample NuGet paketi uygulamanızdaki jQuery sürümü değişebilir, *betikleri* klasör. Yeni *StockTicker.html* paketi yükler dosya *SignalR.Sample* klasörü orijinal çalıştırmakistiyorsanızancakyükleyenpaket,jQuerysürümüileuyumluolacak*StockTicker.html* yeniden dosya, komut dosyası etiketi jQuery başvurusunda önce güncelleştirmeniz gerekebilir.

### <a name="run-the-application"></a>Uygulamayı çalıştırma

 Tablonun ilk uygulamada gördüğünüz kullanışlı özellikler vardı. Yeni özellikler tam borsa uygulama gösterilmektedir: gelmek ve kalan rengini değiştirmek stocks ve stok verilerini gösteren bir yatay kaydırma pencere.

1. Tuşuna **F5** uygulamayı çalıştırmak için.

     Uygulamayı ilk kez çalıştırdığınızda, "pazar" "kapalı" ve statik bir tablo ve kaydırma değil bir bandı penceresi görürsünüz.

1. Seçin **açık Pazar**.

    ![Dinamik sayaç ekran görüntüsü.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * **Canlı hisse senedi bandı** seçenekler yatay kaydırma yapmak ve düzenli aralıklarla hisse senedi fiyatı değişiklikleri rastgele olarak yayınlamak sunucuyu başlatır.

    * Her zaman bir hisse senedi fiyatı değiştirir, hem de uygulama güncelleştirmeleri **hisse senedi tablo canlı** ve **Canlı hisse senedi bandı**.

    * Pozitif bir hisse senedi ait fiyat değişikliği olduğunda, uygulama yeşil bir arka plana sahip stok gösterir.

    * Negatif bir değişiklik olduğunda, uygulama kırmızı bir arka plan ile stok gösterir.

1. Seçin **kapatmak Pazar**.

    * Tablo durdurma güncelleştirir.

    * Bandı kaydırma durdurulur.

1. Seçin **sıfırlama**.

    * Tüm stok verileri sıfırlanır.

    * Uygulamayı kullanmaya fiyat değişiklikleriyle önce ilk durumuna geri yükler.

1. Tarayıcıdan URL'yi kopyalayın, diğer iki tarayıcılar açın ve URL'leri adresi çubukları yapıştırın.

1. Her tarayıcıda aynı anda dinamik olarak güncelleştirilen aynı verileri görürsünüz.

1. Denetimlerden herhangi birini seçtiğinizde, tüm tarayıcılar aynı anda aynı şekilde yanıt verin.

### <a name="live-stock-ticker-display"></a>Canlı hisse senedi bandı görüntüleme

**Canlı hisse senedi bandı** görüntülenir, sırasız bir listesini bir `<div>` öğesi tek bir satıra CSS stillerinden biçimlendirilmiş. Uygulama başlatır ve bandı tablo aynı şekilde güncelleştirir: içindeki yer tutucuları değiştirerek bir `<li>` şablonu dizesi ve dinamik olarak ekleme `<li>` öğelerine `<ul>` öğesi. JQuery kullanarak kaydırma uygulamasını içeren `animate` sırasız liste içinde sol kenar boşluğu değişen işlevi `<div>`.

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

HTML kod borsa kodu:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

CSS kodunu borsa kodu:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

Kolaylaştırır jQuery kodu kaydırın:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>İstemci çağırabilirsiniz sunucuda ek yöntemleri

Uygulamaya esneklik kazandırmak için uygulamayı çağırabilir ek yöntemleri vardır.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

`StockTickerHub` Sınıfı istemci çağırabilirsiniz dört ek yöntemleri tanımlar:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

Uygulama çağrıları `OpenMarket`, `CloseMarket`, ve `Reset` yanıt sayfanın üstündeki düğmeleri olarak. Bunlar, tüm istemcilere hemen yayılan durumundaki bir değişikliği tetikleyen bir istemci desenini gösterir. Bu yöntemlerin her biri bir yöntem çağrıları `StockTicker` Pazar durum değişikliği neden olur ve ardından yeni durum yayınlar sınıfı.

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

İçinde `StockTicker` sınıfı, uygulama ile Pazar durumunu korur bir `MarketState` döndüren özellik bir `MarketState` sabit listesi değeri:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Pazar durum değişikliği yöntemlerin her biri bunu kilit bloğu içinde olduğundan `StockTicker` sınıfında iş parçacığı açısından güvenli olmak üzere:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Bu kod, iş parçacığı açısından güvenli olduğundan emin olmak için `_marketState` yedekler alan `MarketState` özelliği belirlenmiş `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

`BroadcastMarketStateChange` Ve `BroadcastMarketReset` dışında istemcide tanımlanan farklı yöntemleri çağırma yöntemleri zaten gördüğünüz BroadcastStockPrice yöntemine benzer:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Sunucu çağırabilirsiniz istemcide ek işlevler

`updateStockPrice` İşlevi artık işler borsa kodu görüntüleme ve tablodan ve kullandığı `jQuery.Color` kırmızı ve yeşil renk Flash.

Yeni işlevleri *SignalR.StockTicker.js* etkinleştirin ve Pazar durumuna bağlı düğmeleri devre dışı bırakın. Bunlar ayrıca durdurmak veya başlatmak **Canlı hisse senedi bandı** yatay kaydırma. Birçok işlev için eklendiği `ticker.client`, uygulamanın kullandığı [jQuery işlevi genişletmek](http://api.jquery.com/jQuery.extend/) bunları eklemek için.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Bağlantı kurulduktan sonra ek istemci kurulumu

İstemci bağlantı kurar sonra yapmak için bazı ek işleri vardır:

* Pazar açık veya kapalı uygun çağırmak için olup olmadığını öğrenmek `marketOpened` veya `marketClosed` işlevi.

* Düğmeleri sunucu yöntemi çağrıları ekleyin.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Uygulama bağlantı kurar sonra sunucu yöntemleri kadar düğmeleri kadar kablolu değildir. Kullanılabilir gelmeden önce kod server yöntemlerini çağıramazsınız olduğundan.

## <a name="additional-resources"></a>Ek kaynaklar

Bu öğreticide iletileri sunucudan bağlanan tüm istemciler için yayınlar bir SignalR uygulama programı öğrendiniz. Artık düzenli aralıklarla ve herhangi bir istemciden gelen bildirimlere yanıt iletilerini yayınlayabilirsiniz. Çok iş parçacıklı tekil örneğini kavramı, çok oyunculu çevrimiçi oyun senaryolarda sunucu durumunu korumak üzere kullanabilirsiniz. Bir örnek için bkz. [ShootR game SignalR tabanlı](https://github.com/NTaylorMullen/ShootR).

Eşler arası iletişim senaryolarında gösteren öğreticiler için bkz. [SignalR ile çalışmaya başlama](introduction-to-signalr.md) ve [SignalR ile gerçek zamanlı güncelleştirme](tutorial-high-frequency-realtime-with-signalr.md).

SignalR hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [ASP.NET SignalR](../../index.md)
* [SignalR projesi](http://signalr.net/)
* [SignalR GitHub ve örnekleri](https://github.com/SignalR/SignalR)
* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Proje oluşturuldu
> * Sunucu kodunu ayarlayın
> * Sunucu kodu incelenir.
> * İstemci kodunu ayarlayın
> * İstemci dio
> * Uygulamayı test etme
> * Etkin günlüğe kaydetme

ASP.NET SignalR 2 kullanan bir gerçek zamanlı web uygulamasının nasıl oluşturulacağını öğrenmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [SignalR ile gerçek zamanlı bir web uygulaması oluşturma](real-time-web-applications-with-signalr.md)