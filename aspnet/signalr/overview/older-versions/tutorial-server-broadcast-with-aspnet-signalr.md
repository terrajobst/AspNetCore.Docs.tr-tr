---
uid: signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
title: 'Öğretici: ASP.NET SignalR ile sunucu yayını 1.x | Microsoft Docs'
author: pfletcher
description: Bu öğreticide, sunucu yayın işlevselliği sağlamak için ASP.NET SignalR kullanan bir web uygulaması oluşturma işlemi gösterilmektedir. Sunucu anlamına gelir, communic yayını...
ms.author: riande
ms.date: 04/10/2013
ms.assetid: ab7b2554-956a-4f6d-b2a0-4ae0c62e8580
msc.legacyurl: /signalr/overview/older-versions/tutorial-server-broadcast-with-aspnet-signalr
msc.type: authoredcontent
ms.openlocfilehash: 7d604af11ade848358e291d1df25f2f8df850132
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911987"
---
<a name="tutorial-server-broadcast-with-aspnet-signalr-1x"></a>Öğretici: ASP.NET SignalR ile sunucu yayını 1.x
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Bu öğreticide, sunucu yayın işlevselliği sağlamak için ASP.NET SignalR kullanan bir web uygulaması oluşturma işlemi gösterilmektedir. Sunucu yayın istemcilere gönderilen iletişimleri sunucu tarafından başlatılır anlamına gelir. Bu senaryo, eşler arası senaryoları istemcilere iletişimler bir veya daha fazla istemcileri tarafından başlatılır, sohbet uygulamaları gibi daha farklı bir programlama yaklaşım gerektirir.
> 
> Bu öğreticide oluşturacağınız uygulama bir bandı yayın sunucusu işlevselliği için tipik bir senaryo benzetimini yapar.
> 
> Öğreticiyi hakkındaki yorumları davetlidir. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Genel Bakış

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet paketini Visual Studio projesinde benzetimli örnek borsa uygulaması yükler. Bu öğreticinin ilk bölümünde sıfırdan uygulama basitleştirilmiş bir sürümünü oluşturacaksınız. Öğreticinin geri kalanında, NuGet paketini yüklemek ve onu oluşturan kodu ve ek özellikleri gözden geçirin.

Bir "gönderme" veya yayın bildirimleri için düzenli aralıklarla bağlanan tüm istemciler için sunucudan istediğiniz gerçek zamanlı uygulama türünün başvuruda borsa uygulamasıdır.

Bu öğreticinin ilk bölümünde oluşturacağınız uygulama stok verileri içeren bir kılavuz görüntüler.

![StockTicker ilk sürümü](tutorial-server-broadcast-with-aspnet-signalr/_static/image1.png)

Düzenli aralıklarla sunucu rastgele hisse senedi fiyatlarına güncelleştirir ve tüm bağlı istemcileri güncelleştirmeleri gönderir. Tarayıcı sayıları ve sembolleri **değiştirme** ve **%** sütunları dinamik olarak değiştirme sunucudan yanıt bildirimleri. Aynı URL'ye ek tarayıcılar açarsanız, bunların tümü aynı verilere ve verilere aynı değişiklikleri aynı anda gösterin.

Bu öğreticide, aşağıdaki bölümleri içerir:

- [Önkoşullar](#prerequisites)
- [Proje oluşturma](#createproject)
- [SignalR NuGet paketleri Ekle](#nugetpackages)
- [Sunucu kodunu ayarlayın](#server)
- [İstemci kodunu ayarlayın](#client)
- [Uygulamayı test etme](#test)
- [Günlüğe kaydetmeyi etkinleştirme](#enablelogging)
- [Yükleme ve tam StockTicker örneği gözden geçirin](#fullsample)
- [Sonraki adımlar](#nextsteps)

> [!NOTE]
> Uygulama oluşturma adımlarında size çalışmak istemiyorsanız SignalR.Sample paketini yeni bir yükleyebilirsiniz **boş bir ASP.NET Web uygulaması** proje ve kod açıklamaları almak için bu adımları okuyun. Öğreticinin ilk bölümünde bir alt kümesini SignalR.Sample kod kapsar ve ikinci bölümü ek işlevsellik SignalR.Sample paketteki anahtar özelliklerini açıklar.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce sahip olduğunuz Visual Studio 2012 veya 2010 SP1'in bilgisayarınızda yüklü olduğundan emin olun. Visual Studio yoksa bkz [ASP.NET indirir](https://www.asp.net/downloads) ücretsiz Visual Studio Express 2012 için Web alınamıyor.

Visual Studio 2010 varsa, emin [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklenir.

<a id="createproject"></a>

## <a name="create-the-project"></a>Projeyi oluşturma

1. Gelen **dosya** menüsünü tıklatın **yeni proje**.
2. İçinde **yeni proje** iletişim kutusunda **C#** altında **şablonları** seçip **Web**.
3. Seçin **ASP.NET boş Web uygulaması** şablon, proje adı *SignalR.StockTicker*, tıklatıp **Tamam**.

    ![Yeni Proje iletişim kutusu](tutorial-server-broadcast-with-aspnet-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-nuget-packages"></a>SignalR NuGet paketleri Ekle

### <a name="add-the-signalr-and-jquery-nuget-packages"></a>SignalR ve JQuery NuGet paketleri Ekle

Bir NuGet paketini yükleme yoluyla projeye SignalR işlevleri ekleyebilirsiniz.

1. Tıklayın **araçları | NuGet Paket Yöneticisi | Paket Yöneticisi Konsolu**.
2. Paket Yöneticisi'nde aşağıdaki komutu girin.

    [!code-powershell[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample1.ps1)]

    SignalR paket bağımlılıkları olarak birkaç NuGet paketi yükler. Yükleme tamamlandığında tüm SignalR bir ASP.NET uygulamasında kullanmak için gerekli istemci ve sunucu bileşenlerini sahip.

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Sunucu kodunu ayarlayın

Bu bölümde sunucuda çalışan kodu ayarlayın.

### <a name="create-the-stock-class"></a>Stok sınıfı oluşturma

Depolama ve stok hakkında bilgi iletmek için kullanacağınız hisse senedi model sınıfı oluşturarak başlayın.

1. Proje klasöründe yeni bir sınıf dosyası oluşturun, adlandırın *Stock.cs*ve sonra şablon kodunu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample2.cs)]

    ' % S'sembolü (örneğin, Microsoft MSFT) ve fiyat, stocks oluştururken ayarladığınız iki özellikler verilmiştir. Diğer özellikler, nasıl ve ne zaman fiyat ayarlamak bağlıdır. Fiyat, ayarladığınız ilk kez yayıldığı DayOpen için. Ne zaman fiyatı, değişiklik kümesi ve YüzdeDeğişiklikleri özellik değerlerini hesaplanan sonraki kez DayOpen ile fiyatı arasındaki fark temel.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>StockTicker ve StockTickerHub sınıfları oluşturma

SignalR hub'ı API sunucusu istemci etkileşimi işlemek için kullanacaksınız. SignalR hub'ı sınıfından türetilen StockTickerHub sınıfı bağlantıları ve yöntem çağrıları istemcilerinden gelen işler. Stok verileri korumak ve düzenli aralıklarla istemci bağlantıları bağımsız olarak fiyat güncelleştirmeleri tetiklemek için bir zamanlayıcı nesnesi çalıştırmak gerekir. Hub örneği geçici olmadığından bu işlevler bir Hub sınıfta konulamaz. Bir Hub örneği, bağlantılar ve istemciden sunucuya çağrılar gibi hub'ında her işlem için oluşturulur. Bu nedenle StockTicker ad, ayrı bir sınıf içinde çalıştırmak stok verileri tutar, fiyatları güncelleştirir ve fiyat güncelleştirmeleri yayınlar mekanizması vardır.

![Yayın StockTicker gelen](tutorial-server-broadcast-with-aspnet-signalr/_static/image4.png)

Yalnızca her StockTickerHub örneğinden tekil StockTicker örneğe bir başvuru ayarlanacak ihtiyacınız olacak şekilde sunucuda çalıştırılacak StockTicker sınıfının bir örneğini istersiniz. Stok verileri içeren ve güncelleştirmeleri tetiklenir, ancak StockTicker Hub sınıfına değil çünkü istemcilere yayınlayabileceksiniz StockTicker sınıfı var. SignalR hub'ı bağlantı kapsamı nesnesine bir başvuru almak, bu nedenle, StockTicker sınıfı vardır. Bunu daha sonra SignalR bağlantı bağlam nesnesi istemcilere yayınlamak için kullanabilirsiniz.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve tıklayın **Yeni Öğe Ekle**.
2. Visual Studio 2012 ile varsa [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=279941), tıklayın **Web** altında **Visual C#** seçip **SignalR Hub sınıfı** öğe şablonu. Aksi takdirde seçin **sınıfı** şablonu.
3. Yeni bir sınıf adı *StockTickerHub.cs*ve ardından **Ekle**.

    ![StockTickerHub.cs ekleyin](tutorial-server-broadcast-with-aspnet-signalr/_static/image5.png)
4. Şablon kodunu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample3.cs)]

    [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) sınıfı, istemcilerin, sunucuya çağırabilir yöntemleri tanımlamak için kullanılır. Bir yöntem tanımladığınız: `GetAllStocks()`. Bir istemci sunucuya ilk kez bağlandığında, geçerli fiyatları ile stocks tümünün listesini almak için bu yöntemi çağırır. Yöntem zaman uyumlu yürütme ve dönüş `IEnumerable<Stock>` nedeniyle bellekten veri döndürüyor. Yöntem bir veritabanı araması veya bir web hizmeti çağrısı gibi bir bekleme kapsayacaktır bir şey yaparak verileri almak sahip olduğu belirtirsiniz `Task<IEnumerable<Stock>>` zaman uyumsuz işleme etkinleştirmek için dönüş değeri. Daha fazla bilgi için [ASP.NET SignalR Hubs API Kılavuzu - sunucu - zaman zaman uyumsuz olarak yürütülecek](index.md).

    İstemci üzerinde JavaScript kodunda Hub'ın nasıl başvurulacağını HubName özniteliğini belirtir. Bu öznitelik kullanmazsanız, istemcideki varsayılan adı stockTickerHub bu durumda olabilecek bir sınıf adı ortası büyük küçük harfleri bir sürümü var.

    StockTicker sınıfı oluşturduğunuzda, daha sonra göreceğiniz üzere, bu sınıfın tekil örneğini statik örneği özelliğinde oluşturulur. Tekil örneğini StockTicker kaç adet istemcinin bağlanın veya bağlantıyı kesin ne olursa olsun bellekte kalır ve bu örneğe GetAllStocks metodu geçerli bir stok bilgilerini döndürmek için kullandığı olduğunu.
5. Proje klasöründe yeni bir sınıf dosyası oluşturun, adlandırın *StockTicker.cs*ve sonra şablon kodunu aşağıdaki kodla değiştirin:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample4.cs)]

    Birden çok iş parçacığı StockTicker kod'ın aynı örneğinin çalışacağı beri StockTicker sınıfı threadsafe olması gerekir.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Statik bir alana tekil örneğini depolama

    Statik kod başlatır \_örneğiyle sınıf ve bu örnek özelliği yedekleyen örneği alandır oluşturulabilir, sınıfın tek örneğine Oluşturucu, özel olarak işaretlendiğinden. [Yavaş başlatma](https://msdn.microsoft.com/library/dd997286.aspx) için kullanılan \_değildir ancak örnek oluşturma threadsafe olmasını sağlamak üzere performansı artırmak için örnek alanı.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample5.cs)]

    StockTickerHub sınıfında daha önce anlatıldığı gibi bir istemci sunucuya her bağlandığında, ayrı bir iş parçacığı çalıştırırken StockTickerHub sınıfının yeni bir örneğini StockTicker.Instance statik özelliğinden StockTicker tekil örneğini alır.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Bir ConcurrentDictionary stok verileri depolama

    Oluşturucu başlatır \_stocks koleksiyonuyla bazı örnek stok verileri ve GetAllStocks stocks döndürür. Daha önce bahsettiğim gibi stocks Bu koleksiyonu bir sunucu yöntemi istemciler çağırabilirsiniz Hub sınıfta olan StockTickerHub.GetAllStocks tarafından sırayla döndürülür.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample6.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample7.cs)]

    Hisse koleksiyon olarak tanımlanan bir [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) türü için iş parçacığı güvenliği. Alternatif olarak, kullanabileceğinizi bir [sözlük](https://msdn.microsoft.com/library/xfhwa508.aspx) nesne ve ona değişiklikler yaptığınızda sözlük açıkça kilitleme.

    Bu örnek uygulama için bu Tamam bellekte uygulama verilerini depolamak için ve StockTicker örneği çıkarıldığından, veri kaybına olur. Gerçek bir uygulamada bir veritabanı gibi bir arka uç veri deposu ile işe yarar.

    ### <a name="periodically-updating-stock-prices"></a>Hisse senedi fiyatlarına düzenli aralıklarla güncelleştiriliyor

    Düzenli aralıklarla hisse senedi fiyatlarına rastgele olarak güncelleştiren yöntemleri çağıran bir zamanlayıcı nesnesi oluşturan Oluşturucu başlatır.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample8.cs)]

    UpdateStockPrices state parametresi null geçirir. Zamanlayıcı tarafından çağrılır. Fiyatlar güncelleştirmeden önce bir kilit alınmış \_updateStockPricesLock nesne. Kod, başka bir iş parçacığı zaten fiyatları güncelleştiriyor ve ardından listedeki her stoktaki TryUpdateStockPrice çağırır denetler. TryUpdateStockPrice yöntemi hisse senedi fiyatının değiştirip değiştirmemeye karar verir ve değiştirmek için ne kadar. Hisse senedi fiyatının değişirse, hisse senedi fiyatı değişiklik bağlanan tüm istemciler için yayın için BroadcastStockPrice çağrılır.

    \_UpdatingStockPrices bayrak olarak işaretlenmiş [geçici](https://msdn.microsoft.com/library/x13ttww7.aspx) threadsafe erişimi olmasını sağlamak için.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample9.cs)]

    Gerçek bir uygulamada TryUpdateStockPrice yöntemi fiyatı aramak için bir web hizmeti çağırırsınız; Bu kodda değişiklik rastgele bir rasgele sayı üreteci kullanır.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Böylece StockTicker sınıfı istemcilere yayınlayabilirsiniz SignalR bağlamı alma

    Fiyat değişiklikleri buraya StockTicker nesnesinde kaynaklı olduğundan bağlanan tüm istemciler updateStockPrice yöntemi çağırmak için gereken nesne budur. Bir Hub sınıfta bir API İstemci yöntemleri çağırmak için sahip olduğunuz, ancak StockTicker Hub sınıfından türemiyor ve herhangi bir Hub nesnesine bir başvuru yok. Bu nedenle, bağlı istemciler için yayın için StockTickerHub sınıf için SignalR bağlam örneği alın ve istemcilerde yöntemleri çağırmak için kullanmaya StockTicker sınıfı vardır.

    Başvuran geçişleri oluşturucuya tekil sınıfı örneğini oluşturduğunda, kod SignalR bağlamı için bir başvuru alır ve isteğe bağlı olarak Oluşturucu istemciler özelliğinde koyar.

    Yalnızca bir kez bağlamı almak istediğiniz neden iki nedeni vardır: pahalı bir işlem olan içerik alma ve bir kez alma istemcilere gönderilen iletilerin hedeflenen sırasının korunmasını sağlar.

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample10.cs)]

    Bağlam istemciler özellik alma ve StockTickerClient özelliğinde koyarak olduğu gibi aynı görünen bir Hub sınıfta istemci yöntemleri çağırmak için kod yazmanıza olanak sağlar. Örneğin, tüm istemcilere yayın Clients.All.updateStockPrice(stock) yazabilirsiniz.

    İçinde BroadcastStockPrice aradığınız updateStockPrice yöntemi henüz mevcut değil; istemcide çalışan kod yazdığınızda, daha sonra ekleyeceksiniz. Çalışma zamanında hesaplanacak olan ifade anlamına gelen dinamik Clients.All olduğu için burada updateStockPrice başvurabilir. Bu yöntem çağrısının yürütüldüğünde, SignalR yöntem adı ile parametre değeri istemciye gönderir ve istemci updateStockPrice adlı bir yöntemi varsa, bu yöntem çağrılır ve parametre değeri geçirilir.

    Clients.All tüm istemcilere göndermek anlamına gelir. SignalR, hangi istemcilerin veya göndermek için istemci gruplarının belirtmek için diğer seçenekler sunar. Daha fazla bilgi için [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>SignalR yol kaydetme

Sunucunun kesecek ve SignalR ile doğrudan hangi URL'sini de bilmeniz gerekir. İçin bazı kodlar ekleyeceksiniz yapmak için *Global.asax* dosya.

1. İçinde **Çözüm Gezgini**projeye sağ tıklayın ve ardından **Yeni Öğe Ekle**.
2. Seçin **genel uygulama sınıfı** öğe şablonu ve ardından **Ekle**.

    ![Global.asax Ekle](tutorial-server-broadcast-with-aspnet-signalr/_static/image6.png)
3. SignalR yol kayıt kodu uygulamanıza eklemek\_başlangıç yöntemi:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample11.cs)]

    Varsayılan olarak, tüm SignalR trafiği temel URL'si olan "/ signalr", ve "/ signalr/hubs", uygulamanızda sahip olduğunuz tüm hub'ları proxy'lerini tanımlayan dinamik olarak üretilen bir JavaScript dosyasını almak için kullanılır. Farklı temel URL ile SignalR seçenekler belirli bir örneğini belirtmenize olanak tanıyan aşırı MapHubs yöntemi içeren [HubConfiguration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubconfiguration(v=vs.111).aspx) sınıfı.
4. Kullanarak bir ekleme deyimini dosyanın üst:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample12.cs)]
5. Kaydet ve Kapat *Global.asax* dosya ve projeyi derleyin.

Sunucu kodu ayarlama tamamladınız. Sonraki bölümde, istemci ayarlamanız.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>İstemci kodunu ayarlayın

1. Proje klasöründe yeni bir HTML dosyası oluşturun ve adlandırın *StockTicker.html*.
2. Şablon kodunu aşağıdaki kodla değiştirin:

    [!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample13.html)]

    HTML 5 sütun ve üst bilgi satırı tüm 5 sütun kapsayan tek bir hücre içeren bir veri satırı bir tablo oluşturur. Veri satırı "yükleniyor..." görüntüler ve yalnızca kısa bir süre içinde uygulama başladığında gösterilir. JavaScript kodu satır kaldırın ve sunucudan alınan stok verilerini kendi yerde satır ekleyin.

    Komut dosyası etiketlerini jQuery komut dosyası, SignalR çekirdek komut dosyası, SignalR proxy'leri komut dosyası ve daha sonra oluşturacağınız bir StockTicker betik dosyası belirtin. "/ Signalr/hubs" URL'sini belirtir, SignalR proxy'leri komut dosyası dinamik olarak oluşturulur ve Hub sınıfı yöntemleri için proxy yöntemleri için StockTickerHub.GetAllStocks bu durumda tanımlar. Tercih ederseniz, bu JavaScript dosyasını el ile kullanarak oluşturabileceğiniz [SignalR yardımcı programları](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) ve MapHubs yöntemi çağrısında dinamik dosya oluşturma devre dışı bırakın.
3. > [!IMPORTANT]
   > JavaScript dosyası içinde başvurduğundan emin olun *StockTicker.html* doğrudur. Diğer bir deyişle, komut dosyası etiketi (örnekte 1.8.2) jQuery sürümünde projenizin jQuery sürümüyle aynı olduğundan emin olun *betikleri* klasöründe ve betik etiketinizi SignalR sürümünde SignalR ile aynı olduğundan emin olun projenizin sürüm *betikleri* klasör. Komut dosyası etiketlerini dosya adlarında gerekirse değiştirin.
4. İçinde **Çözüm Gezgini**, sağ *StockTicker.html*ve ardından **Başlangıç Sayfası Ayarla**.
5. Proje klasöründe yeni bir JavaScript dosyası oluşturun ve adlandırın *StockTicker.js*...
6. Şablon kodunu aşağıdaki kodla değiştirin:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample14.js)]

    $.connection SignalR proxy'leri için ifade eder. Kod StockTickerHub sınıf için proxy için bir başvuru alır ve bandı değişkenine yerleştirilir. Ara sunucu adını [HubName] özniteliği tarafından ayarlanan adıdır:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample15.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample16.cs)]

    Tüm değişkenlerin ve işlevlerin tanımlandıktan sonra dosyayı kodda son satırının SignalR başlangıç işlevini çağırarak SignalR bağlantı başlatır. Başlangıç işlev zaman uyumsuz olarak yürütür ve döndürür bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/), bunun anlamı zaman uyumsuz işlem tamamlandığında çağırılacak işlev belirtmek için yapılan işlevi çağırabilir...

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample17.js)]

    İnit işlevi sunucuda getAllStocks işlevini çağırır ve sunucu Stok tablosu döndürür bilgileri kullanır. Yöntem adı sunucu üzerinde pascal harfleri olmasına rağmen camel istemcide büyük/küçük harfleri kullanmak zorunda varsayılan olarak, dikkat edin. Ortası büyük/küçük harf kuralı yalnızca nesneleri yöntemleri için geçerlidir. Örneğin, hisse senedi için bakın. Simge ve stok. Fiyat stock.symbol ya da değil stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample18.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample19.cs)]

    İstemcide pascal casing kullanmak istediğinizi veya tamamen farklı yöntem adı kullanmak istiyorsanız, Hub yönteminin HubMethodName özniteliği ile aynı şekilde tasarlamanız Hub sınıfın kendisi HubName özniteliği ile donatılmış.

    İnit yöntemi stok biçimi özelliklerine göre arama formatStock sunucudan alınan stok her nesne için bir tablo satır için HTML oluşturulur ve ardından çağırarak supplant (üst kısmında tanımlanan *StockTicker.js*) stok özellik değerleri rowTemplate değişkeni içindeki yer tutucuları değiştirmek için. Sonuçta elde edilen HTML sonra stok tablosuna eklenir.

    İnit, içinde zaman uyumsuz başlangıç işlevi tamamlandıktan sonra yürüten bir geri çağırma işlevi olarak geçirerek çağırın. Başlangıç çağrıldıktan sonra ayrı bir JavaScript ifadesi olarak init çağrılırsa, bağlantı kurma tamamlanması için başlangıç işlevi beklenmeden hemen yürütülür çünkü işlev başarısız olur. Bu durumda, init işlevi sunucu bağlantısı yeniden kurulmadan önce getAllStocks işlev çağrısı isteriz.

    Sunucu hisse ait fiyat değiştiğinde, bağlı istemciler üzerinde updateStockPrice çağırır. İşlev çağrıları sunucudan kullanabilmesi için stockTicker proxy istemci özelliğini eklenir.

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample20.js)]

    Sunucudan init işlevi aynı şekilde bir tablo satır alınan bir stok updateStockPrice işlevi biçimlendirir. Ancak, tabloya satır ekleme, yerine tabloda hisse senedi'nın geçerli satırı bulur ve satır yenisiyle değiştirir.

<a id="test"></a>

## <a name="test-the-application"></a>Uygulamayı test etme

1. Uygulamayı hata ayıklama modunda çalıştırmak için F5 tuşuna basın.

    "Satırın ardından ilk stok verileri görüntülenen kısa bir gecikmeyle yükleniyor..." stok tablonun ilk başta görüntüler ve değiştirmek hisse senedi fiyatlarına başlatın.

    ![Yükleniyor](tutorial-server-broadcast-with-aspnet-signalr/_static/image7.png)

    ![İlk stok tablosu](tutorial-server-broadcast-with-aspnet-signalr/_static/image8.png)

    ![Stok tablosu değişiklikleri sunucudan alınıyor](tutorial-server-broadcast-with-aspnet-signalr/_static/image9.png)
2. Tarayıcınızın adres çubuğundan URL'yi kopyalayın ve bir veya daha fazla yeni tarayıcı pencerelerini yapıştırın.

    İlk stok görünen ilk tarayıcı ile aynıdır ve değişiklikler aynı anda gerçekleşir.
3. Tüm tarayıcıları kapatın ve yeni bir tarayıcı açın ve sonra aynı URL'ye gidin.

    StockTicker tekil nesnesi stocks değiştirmeye devam stok tablo gösterecek şekilde sunucu çalışmaya devam etti. (Şekiller değiştirme tablonun ilk sıfır görmüyorum.)
4. Tarayıcıyı kapatın.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Günlüğe kaydetmeyi etkinleştirme

SignalR istemcide giderilmesine yardımcı olmak için etkinleştirebileceğiniz bir yerleşik günlük işleve sahiptir. Bu bölümdeki günlük kaydını etkinleştirmek ve nasıl günlükleri SignalR kullanarak, aşağıdaki taşıma yöntemleri size gösteren örneklere bakın:

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), IIS 8 ve geçerli tarayıcılar tarafından desteklenir.
- [Sunucu tarafından gönderilen olayları](http://en.wikipedia.org/wiki/Server-sent_events), Internet Explorer dışındaki tarayıcıları tarafından desteklenmiyor.
- [Sonsuza kadar çerçeve](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), Internet Explorer tarafından desteklenmiyor.
- [AJAX uzun yoklama](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), tüm tarayıcılar tarafından desteklenir.

Verilen herhangi bir bağlantısı için hem sunucu hem de istemci destekleyen en iyi bir aktarım yöntemi SignalR seçer.

1. Açık *StockTicker.js* ve bir dosyanın sonuna bağlantı başlatır kodundan hemen önce günlüğe kaydetmeyi etkinleştirmek için kod satırını ekleyin:

    [!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample21.js)]
2. Projeyi çalıştırmak için F5 tuşuna basın.
3. Tarayıcınızın geliştirici araçları penceresini açın ve konsol günlükleri görmek için seçin. Signalr aktarım yöntemi için yeni bir bağlantı anlaşması günlükleri görmek için sayfayı yenilemeniz gerekebilir.

    Internet Explorer 10, Windows 8'de (IIS 8) çalıştırıyorsanız, aktarım WebSockets yöntemidir.

    ![IE 10 IIS 8 Konsolu](tutorial-server-broadcast-with-aspnet-signalr/_static/image10.png)

    Internet Explorer 10, Windows 7'de (IIS 7.5) çalıştırıyorsanız, aktarım iframe yöntemidir.

    ![IE 10 konsolu, IIS 7.5](tutorial-server-broadcast-with-aspnet-signalr/_static/image11.png)

    ' De Firefox, Firebug bir konsol penceresi almak için eklentisini yükleyin. Windows 8 (IIS 8)'de Firefox 19 çalıştırıyorsanız, aktarım WebSockets yöntemidir.

    ![Firefox 19 8 IIS Websockets](tutorial-server-broadcast-with-aspnet-signalr/_static/image12.png)

    Windows 7 (IIS 7.5)'de Firefox 19 çalıştırıyorsanız, aktarım sunucu tarafından gönderilen olayları yöntemidir.

    ![IIS 7.5 Firefox 19 Konsolu](tutorial-server-broadcast-with-aspnet-signalr/_static/image13.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Yükleme ve tam StockTicker örneği gözden geçirin

Tarafından yüklenen StockTicker uygulama [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) NuGet paketini sıfırdan yeni oluşturduğunuz Basitleştirilmiş sürümden daha fazla özellik içerir. Öğreticinin bu bölümünde NuGet paketini yükleyin ve yeni özellikleri ve bunları uygulayan kod gözden geçirin.

### <a name="install-the-signalrsample-nuget-package"></a>SignalR.Sample NuGet paketini yükle

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve tıklayın **NuGet paketlerini Yönet**.
2. İçinde **NuGet paketlerini Yönet** iletişim kutusu, tıklayın **çevrimiçi**, girin *SignalR.Sample* içinde **çevrimiçi Ara** kutusuna ve ardından tıklatın **Yükleme** içinde **SignalR.Sample** paket.

    ![SignalR.Sample paketini yükle](tutorial-server-broadcast-with-aspnet-signalr/_static/image14.png)
3. İçinde *Global.asax* uygulamada daha önce eklediğiniz satır; dosyası, RouteTable.Routes.MapHubs() yorum\_yöntemi başlatın.

    Kodda *Global.asax* SignalR yolda SignalR.Sample paket kaydeder olduğundan artık gerekli olmadığında *uygulama\_Start/RegisterHubs.cs* dosyası:

    [!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample22.cs)]

    Bütünleştirilmiş kod özniteliği tarafından başvurulan WebActivator sınıfı SignalR.Sample paket bağımlılık olarak yüklü WebActivatorEx NuGet paketini dahildir.
4. İçinde **Çözüm Gezgini**, genişletme *SignalR.Sample* SignalR.Sample paketini yükleyerek oluşturulduğu klasör.
5. İçinde *SignalR.Sample* klasörü sağ tıklatın *StockTicker.html*ve ardından **başlangıç sayfası olarak ayarla**.

    > [!NOTE]
    > Yükleme SignalR.Sample NuGet paketi uygulamanızdaki jQuery sürümü değişebilir, *betikleri* klasör. Yeni *StockTicker.html* paketi yükler dosya *SignalR.Sample* klasörü orijinal çalıştırmakistiyorsanızancakyükleyenpaket,jQuerysürümüileuyumluolacak*StockTicker.html* yeniden dosya, komut dosyası etiketi jQuery başvurusunda önce güncelleştirmeniz gerekebilir.

### <a name="run-the-application"></a>Uygulamayı çalıştırın

1. Uygulamayı çalıştırmak için F5 tuşuna basın.

    Daha önce gördüğünüzle kılavuza ek olarak, tam borsa uygulama aynı stok verileri görüntüleyen bir yatay kaydırma penceresi gösterilmektedir. Uygulamayı ilk kez çalıştırdığınızda, "pazar" "kapalı" ve statik bir kılavuz ve kaydırma değil bir bandı penceresi görürsünüz.

    ![StockTicker ekran Başlat](tutorial-server-broadcast-with-aspnet-signalr/_static/image15.png)

    Tıkladığınızda **açık Pazar**, **Canlı hisse senedi bandı** seçenekler yatay kaydırma yapmak ve düzenli aralıklarla hisse senedi fiyatı değişiklikleri rastgele olarak yayınlamak sunucuyu başlatır. Her zaman bir hisse senedi fiyatı değiştirir, her ikisi de **hisse senedi tablo canlı** kılavuz ve **Canlı hisse senedi bandı** kutusu güncelleştirilir. Hisse ait fiyat değişikliği pozitif olduğunda hisse senedi yeşil bir arka plan ile gösterilir ve değişiklik negatif olduğunda hisse senedi kırmızı bir arka plan ile gösterilir.

    ![Pazar StockTicker uygulamasını açın](tutorial-server-broadcast-with-aspnet-signalr/_static/image16.png)

    **Kapat Pazar** düğmesi değişiklikleri ve kaydırma bandı durur ve **sıfırlama** düğme fiyat değişiklikleriyle başlatmadan önce bu tüm stok verileri ilk durumuna sıfırlar. Daha fazla tarayıcı pencerelerini açmak ve aynı URL'ye gitmeniz her tarayıcıda aynı anda dinamik olarak güncelleştirilen aynı verileri görürsünüz. Düğmelerden birine tıkladığınızda, tüm tarayıcılar aynı anda aynı şekilde yanıt verin.

### <a name="live-stock-ticker-display"></a>Canlı hisse senedi bandı görüntüleme

**Canlı hisse senedi bandı** sırasız bir listesini tek bir satıra CSS stillerinden biçimlendirilmiş bir div öğesine içinde görüntülenir. Bandı başlatılır ve tabloyu aynı şekilde güncelleştirildi: içindeki yer tutucuları değiştirerek bir &lt;li&gt; şablonu dizesi ve dinamik olarak ekleme &lt;li&gt; öğelerine &lt;ul&gt; öğe. Kaydırma DIV içinde sırasız liste sol kenar boşluğu değiştirmek için jQuery Canlandır işlevi kullanılarak gerçekleştirilir

HTML borsa kodu:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample23.html)]

CSS borsa kodu:

[!code-html[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample24.html)]

Kolaylaştırır jQuery kodu kaydırın:

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample25.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>İstemci çağırabilirsiniz sunucuda ek yöntemleri

İstemci çağırabilirsiniz dört ek yöntemleri StockTickerHub sınıfı tanımlar:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample26.cs)]

Sayfanın üstündeki düğmeleri yanıt OpenMarket CloseMarket ve sıfırlama çağrılır. Bunlar, tüm istemcilere hemen yayılır durumundaki bir değişikliği tetikleyen bir istemci desenini gösterir. Bu yöntemlerin her biri bir yöntem StockTicker sınıfında Pazar durumu değiştirin ve ardından yeni durum yayınlar bu etkileri çağırır.

StockTicker sınıfında, Pazar durumunu MarketState enum değeri döndüren bir MarketState özelliği tarafından korunur:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample27.cs)]

Pazar durum değişikliği yöntemlerin her biri bunu kilit bloğu içinde StockTicker sınıfı threadsafe olabilir çünkü:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample28.cs)]

Bu kod, threadsafe olduğundan emin olmak için \_MarketState özelliği yedekleyen marketState alan geçici olarak işaretlendi

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample29.cs)]

Bunlar istemcide tanımlanan farklı yöntemleri çağırmak dışında BroadcastMarketStateChange ve BroadcastMarketReset yöntemleri zaten gördüğünüz BroadcastStockPrice yöntemine benzerdir:

[!code-csharp[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample30.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Sunucu çağırabilirsiniz istemcide ek işlevler

UpdateStockPrice işlevi artık kılavuz hem bandı görüntü işleme ve kırmızı ve yeşil renk Flash jQuery.Color kullanır.

Yeni işlevleri *SignalR.StockTicker.js* etkinleştirme ve devre dışı düğmeleri temel durum pazarlamak ve durdurma veya bandı pencereyi yatay kaydırma başlatın. Birden çok işlevleri ticker.client için eklendiği [jQuery işlevi genişletmek](http://api.jquery.com/jQuery.extend/) bunları eklemek için kullanılır.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample31.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Bağlantı kurulduktan sonra ek istemci kurulumu

İstemci bağlantı kurar sonra yapmak için bazı ek işleri sahip: Pazar açık veya kapalı uygun marketOpened veya marketClosed işlevi çağırabilir ve sunucu yöntem çağrılarını düğmeleri eklemek için olup olmadığını öğrenin.

[!code-javascript[Main](tutorial-server-broadcast-with-aspnet-signalr/samples/sample32.js)]

Bağlantı kurulduktan sonra böylece kullanılabilir olmadan önce sunucu yöntemlerini çağırmak kod deneyemez server yöntemlerini kadar düğmeleri kadar kablolu arabirimlerdir değil.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide sunucudan tüm bağlı istemciler, düzenli aralıklarla ve herhangi bir istemciden gelen bildirimlere yanıt iletileri yayınlar bir SignalR uygulama programı öğrendiniz. Sunucu durumunu korumak üzere bir çok iş parçacıklı tekil örneğini kullanarak desenini ayrıca çok oyunculu çevrimiçi oyun senaryolarda da kullanılabilir. Bir örnek için bkz. [SignalR tabanlı ShootR game](https://github.com/NTaylorMullen/ShootR).

Eşler arası iletişim senaryolarında gösteren öğreticiler için bkz. [SignalR ile çalışmaya başlama](index.md) ve [SignalR ile gerçek zamanlı güncelleştirme](index.md).

Daha gelişmiş SignalR geliştirme kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [ASP.NET SignalR](https://asp.net/signalr/)
- [SignalR projesi](http://signalr.net/)
- [SignalR Github ve örnekleri](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
