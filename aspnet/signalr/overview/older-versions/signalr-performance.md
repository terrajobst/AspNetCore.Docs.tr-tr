---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR performansı (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: SignalR performansı
ms.author: riande
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 4fba0cc79046f5f3fd1dc50e5b69ddb78d98c23d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751980"
---
<a name="signalr-performance-signalr-1x"></a>SignalR performansı (SignalR 1.x)
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

> Bu konuda, SignalR uygulama performansını artırmak için tasarlayın ve ölçmek açıklar.


SignalR performans ve ölçeklendirme hakkında son sunumu için bkz [ASP.NET SignalR ile gerçek zamanlı Web ölçeklendirme](https://channel9.msdn.com/Events/Build/2013/3-502).

Bu konu aşağıdaki bölümleri içermektedir:

- [Tasarım konuları](#design)
- [SignalR sunucunuz için performans ayarlama](#tuning)
- [Performans sorunlarını giderme](#troubleshooting)
- [SignalR performans sayaçları kullanma](#perfcounters)
- [Diğer performans sayaçları kullanma](#othercounters)
- [Diğer kaynaklar](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Tasarım konuları

Bu bölümde performans gereksiz ağ trafiğini oluşturarak engelliyordu değil emin olmak için bir SignalR uygulamasının Tasarım sırasında uygulanan desenler açıklanmaktadır.

### <a name="throttling-message-frequency"></a>İleti sıklığı azaltma

(Örneğin, gerçek zamanlı oyun uygulaması) bir yüksek sıklıkta ileti gönderen bile bir uygulamada, çoğu uygulama, ikinci bir fazla sayıda ileti göndermek gerekmez. Her istemci ürettiği trafik miktarını azaltmak için bir ileti döngüsü kuyruklar ve göndereceğini sık en fazla sabit bir ücrete iletileri uygulanabilir (süre içerisinde iletileri ise diğer bir deyişle, belirli sayıda ileti kadar her saniye gönderilir gönderilecek oluşturma aralığı). İletiler (hem istemci ve sunucu) için belirli bir fiyatı kısıtlar örnek bir uygulama için bkz. [SignalR ile yüksek sıklıkta gerçek zamanlı](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>İleti boyutu azaltma

SignalR ileti boyutu, serileştirilmiş nesneler boyutunu azaltarak azaltabilir. Aktarılması gerekmez özellikleri içeren bir nesne gönderiyorsanız, sunucu kodunda bu özellikleri kullanarak serileştirilen öğesinden önlemek `JsonIgnore` özniteliği. Özellik adlarını da iletide depolanır; Özellik adlarını kullanarak kısalttık `JsonProperty` özniteliği. Aşağıdaki kod örneği, nasıl istemciye gönderilen bir özelliği dışarıda ve özellik adlarını kısaltmak nasıl gösterir:

**İstemciye gönderilen veri dışlanacak JsonIgnore öznitelik ve ileti boyutunu küçültmek için Item özniteliğini gösterir .NET sunucu kodu**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Okunabilirliğini korumak için / bakım istemci kodu içinde kısaltılmış özellik adları olabilir İnsan dostu uri'lerini adları ileti alındıktan sonra. Aşağıdaki kod örneği bir ileti anlaşması (eşleme) tanımlayarak uzun olanlara, kısaltılmış adı yeniden eşleme, olası bir yol gösterir ve kullanarak `reMap` işlevi en iyi duruma getirilmiş ileti sınıfı için sözleşme uygulamak için:

**Özellik adlarının okunabilir adlarına yeniden eşleyen bir istemci tarafı JavaScript kodu kısalttık**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Adları iletilerinde istemci aynı zamanda sunucu için aynı yöntemi kullanarak kısaltılması.

(Diğer bir deyişle, ileti için kullanılan bellek miktarı) bellek kullanım alanını azaltmaya ileti nesne performansını iyileştirebilir. Örneğin, tam aralığının bir `int` gerekli değildir, bir `short` veya `byte` bunun yerine kullanılabilir.

İletileri ileti veri yolunda iletileri boyutunu azaltma, sunucu belleği depolandığından sunucu bellek sorunlarını gidermek de kullanabilirsiniz.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>SignalR sunucunuz için performans ayarlama

Aşağıdaki yapılandırma ayarları, SignalR uygulamada daha iyi performans için sunucunuzu ayarlamak için kullanılabilir. Bir ASP.NET uygulamasında performansını artırma hakkında genel bilgi için bkz. [ASP.NET performans geliştirme](https://msdn.microsoft.com/library/ff647787.aspx).

**SignalR yapılandırma ayarları**

- **DefaultMessageBufferSize**: varsayılan olarak, SignalR hub'ı bağlantı başına başına bellek 1000 iletilerinde korur. Büyük iletileri kullanılıyorsa, bu, bu değer azaltarak alleviated bellek sorunlarını oluşturabilir. Bu ayar ayarlanabilir `Application_Start` olay işleyicisi bir ASP.NET uygulaması ya da `Configuration` yöntemi OWIN başlangıç sınıfı şirket içinde barındırılan bir uygulamada. Aşağıdaki örnek, kullanılan sunucu bellek miktarını azaltmak için uygulamanızın bellek ayak izini azaltmak için bu değeri azaltmanız gösterilmektedir:

    **Varsayılan ileti arabellek boyutunu azaltmak için .NET sunucu kodunda Global.asax**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**IIS yapılandırma ayarları**

- **Uygulama başına en fazla eş zamanlı istek**: eş zamanlı IIS sayısının artırılması istekleri isteklerine hizmet için kullanılabilir sunucu kaynaklarına artacaktır. Varsayılan değer 5000'dir; Bu ayar artırmak için yükseltilmiş bir komut isteminde aşağıdaki komutları yürütün:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**ASP.NET yapılandırma ayarları**

Bu bölümü, ayarlanabilen yapılandırma ayarlarını içerir `aspnet.config` dosya. Bu dosya, platforma bağlı olarak iki konumlardan birinde bulunur:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

SignalR performansı iyileştirebilir ASP.NET ayarları aşağıdakileri içerir:

- **CPU başına en fazla eş zamanlı istek**: Bu ayar artan performans sorunları hafifletmek. Bu ayar artırmak için aşağıdaki yapılandırma ayarı ekleme `aspnet.config` dosyası:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Sıra sınırı iste**: toplam bağlantı sayısını aştığında `maxConcurrentRequestsPerCPU` ayarlama, ASP.NET bir kuyruk kullanma istekleri azaltma başlar. Sıranın boyutunu artırmak için artırabilirsiniz `requestQueueLimit` ayarı. Bunu yapmak için aşağıdaki yapılandırma ayarı ekleme `processModel` düğümünde `config/machine.config` (yerine `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Performans sorunlarını giderme

Bu bölümde, uygulamanızda performans sorunlarını bulmak için yolları açıklanmaktadır.

### <a name="verifying-that-websocket-is-being-used"></a>WebSocket kullanılmakta olduğunu doğrulama

SignalR istemci ve sunucu arasındaki iletişim için taşımalar çeşitli kullanabilirsiniz, ancak WebSocket önemli performans avantajı sunar ve istemci ve sunucu destekliyorsa kullanılmalıdır. İstemci ve sunucu WebSocket gereksinimlerini karşılayıp karşılamadığını belirlemek için bkz: [aktarım ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports). Hangi aktarım uygulamanızda kullanıldığını belirlemek için tarayıcı geliştirici araçlarını kullanın ve aktarım bağlantı için kullanıldığını görmek için günlüklerini inceleyin. Internet Explorer ve Chrome tarayıcı geliştirme araçları kullanma hakkında daha fazla bilgi için bkz. [aktarım ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>SignalR performans sayaçları kullanma

Bu bölümde, etkinleştirmek ve kullanmak SignalR performans sayaçları açıklanmaktadır bulunan `Microsoft.AspNet.SignalR.Utils` paket.

### <a name="installing-signalrexe"></a>Signalr.exe yükleme

Performans sayaçları SignalR.exe adlı bir yardımcı program kullanarak sunucuya eklenebilir. Bu yardımcı programını yüklemek için aşağıdaki adımları izleyin:

1. Visual Studio uygulamanızı seçin **Araçları**, **kitaplık Paket Yöneticisi**, **çözüm için NuGet paketlerini Yönet...**
2. Arama **signalr.utils**ve yükleme seçeneğini belirleyin.

    ![](signalr-performance/_static/image1.png)
3. Paketi yüklemek için lisans sözleşmesini kabul edin.
4. SignalR.exe için yüklenecek `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Performans sayaçları ile SignalR.exe yükleniyor

SignalR performans sayaçlarını yüklemek için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

SignalR performans sayaçları kaldırmak için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>SignalR performans sayaçları

Aşağıdaki performans sayaçları yardımcı programlar paketi yükler. Son uygulama havuzunu veya sunucu yeniden başlatıldığından beri "Toplam" sayaçları, olay sayısını ölçer.

**Bağlantı ölçümü**

Aşağıdaki ölçümler gerçekleşen bağlantı ömrü olaylarını ölçün. Daha fazla bilgi için [anlama ve işleme bağlantı ömrü olaylarını](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Bağlı bağlantıları**
- **Bağlantıları yeniden bağlantı kuruldu**
- **Bağlantıları Disonnected**
- **Bağlantılar-geçerli**

**İleti ölçümleri**

Aşağıdaki ölçümler SignalR tarafından oluşturulan ileti trafiği ölçer.

- **Alınan toplam bağlantı iletisi**
- **Toplam gönderilen bağlantısı iletileri**
- **Bağlantı alınan ileti sayısı/sn**
- **Bağlantı gönderilen ileti sayısı/sn**

**Message bus ölçümleri**

Aşağıdaki ölçümler, iç SignalR ileti veri, tüm gelen ve giden SignalR iletileri yerleştirilir kuyruk üzerinden geçen trafik ölçün. Bir ileti **yayımlanan** zaman bunu gönderilir ya da yayın. A **abone** bu bağlamda bir ileti veri yoluna abonelikte; bu istemciler ve sunucu sayısına eşit. Bir **ayrılmış çalışan** veri gönderen etkin bağlantılar için; bir bileşeni bir **meşgul çalışan** , etkin bir ileti gönderen bir uygulamadır.

- **Alınan toplam ileti veri yoluna iletileri**
- **İleti veri yoluna iletileri/sn**
- **Toplam ileti veri yoluna yayımlanan**
- **İleti veri yoluna iletileri yayımlanan/sn**
- **İleti veri yoluna abone geçerli**
- **İleti veri yoluna abone toplam**
- **İleti veri yoluna abone/sn**
- **İleti veri yoluna ayrılan çalışanları**
- **İleti veri yolu meşgul çalışanların**
- **İleti veri yolu konuları geçerli**

**Hata ölçümleri**

SignalR ileti trafiği tarafından oluşturulan hatalar aşağıdaki ölçümleri ölçün. **Hub çözümleme** hataları bir hub veya hub'yöntemini çözümlenemiyor olduğunda oluşur. **Hub çağırma** hataları olan bir hub yöntemi çağrılırken oluşan özel durum. **Aktarım** bir HTTP isteği veya yanıtı sırasında oluşturulan bağlantı hataları hatalardır.

- **Hataları: Tüm toplam**
- **Hata: All/sn**
- **Hataları: Hub çözümleme toplam**
- **Hata: Hub çözümleme/sn**
- **Hataları: Hub çağırma toplam**
- **Hata: Hub çağırma/sn**
- **Hata: Aktarım toplam**
- **Hata: Aktarım/sn**

**Genişletme ölçümleri**

Aşağıdaki ölçümler, trafik ve genişletme sağlayıcı tarafından oluşturulan hatalar ölçün. A **Stream** bu bağlamda bir ölçek birimi genişletme sağlayıcı tarafından kullanılan; SQL Server kullandıysanız bir tablo, Service Bus kullanılırsa, bir konu ve abonelik Redis kullandıysanız budur. Varsayılan olarak, yalnızca bir akış kullanılır, ancak bu yapılandırma, SQL Server ve Service Bus aracılığıyla artırılabilir. A **arabelleği** stream, hatalı bir duruma geçtiğini; akış hatalı bir durumda olduğunda hemen akış artık hatalı kadar devre kartına gönderilen tüm iletilerin başarısız olur. **Gönderme sırası uzunluğu** gönderilen, ancak henüz gönderilen iletilerin sayısı.

- **Genişletme ileti veri yoluna iletileri/sn**
- **Genişletme akışları toplam**
- **Genişletme açık akışları**
- **Genişletme akışları arabelleğe alma**
- **Genişletme hataları toplamı**
- **Genişletme Hatası/sn**
- **Genişletme gönderme sırası uzunluğu**

Ne Bu sayaçlar ölçme ile ilgili daha fazla bilgi için bkz: [Azure Service Bus ile SignalR ölçeğini genişletme](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Diğer performans sayaçları kullanma

Aşağıdaki performans sayaçları, uygulamanızın performansını izlemek için yararlı olabilir.

**Bellek**

- .NET CLR bellek # bayt cinsinden tüm yığınlardaki (w3wp)

**ASP.NET**

- ASP.NET\Requests geçerli
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- İşlemci Information\Processor zamanı

**TCP/IP**

- TCPv6/kurulan bağlantılar
- TCPv4/kurulan bağlantılar

**Web hizmeti**

- Web Service\Current bağlantıları
- Web Service\Maximum bağlantıları

**İş parçacığı oluşturma**

- .NET CLR LocksAndThreads\# geçerli mantıksal iş parçacığı sayısı
- .NET CLR LocksAnd iş parçacıkları\# geçerli fiziksel iş parçacığı sayısı

<a id="otherresources"></a>

## <a name="other-resources"></a>Diğer Kaynaklar

ASP.NET Performans izleme ve ayarlama hakkında daha fazla bilgi için aşağıdaki konulara bakın:

- [ASP.NET performansa genel bakış](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [IIS 6.0 IIS 7.5 ve IIS 7.0 üzerinde ASP.NET iş parçacığı kullanımı](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; öğesi (Web Ayarları)](https://msdn.microsoft.com/library/dd560842.aspx)
