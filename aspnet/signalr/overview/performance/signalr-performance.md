---
uid: signalr/overview/performance/signalr-performance
title: SignalR performans | Microsoft Docs
author: pfletcher
description: SignalR performans
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: dec2602e47fbcb838643a506a7e3feebda9d9c81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance"></a>SignalR performans
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

> Bu konuda, bir SignalR uygulama performansını artırmak için tasarım ve ölçmek açıklar.
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


SignalR performans ve ölçeklendirme son sunumu için bkz: [ASP.NET SignalR ile gerçek zamanlı Web ölçeklendirme](https://channel9.msdn.com/Events/Build/2013/3-502).

Bu konu aşağıdaki bölümleri içermektedir:

- [Tasarım konuları](#design)
- [SignalR sunucunuz için performans ayarlama](#tuning)
- [Performans sorunlarını giderme](#troubleshooting)
- [SignalR performans sayaçlarını kullanma](#perfcounters)
- [Diğer performans sayaçlarını kullanma](#othercounters)
- [Diğer kaynaklar](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Tasarım konuları

Bu bölüm performans gereksiz ağ trafiğini oluşturarak engelliyordu değil emin olmak için bir SignalR uygulamasının Tasarım sırasında uygulanan desenleri açıklar.

### <a name="throttling-message-frequency"></a>İleti sıklığı azaltma

Yüksek yoğunlukta (örneğin, gerçek zamanlı oyun uygulaması) iletileri gönderir bile bir uygulamada, ikinci bir birkaç iletileri göndermek uygulamaların çoğu gerekmez. Her istemci ürettiği trafik miktarını azaltmak için bir ileti döngüsü kuyruklar ve göndereceğini en sık sabit oranı çok iletileri uygulanabilir (Bu süre içinde iletileri ise diğer bir deyişle, belirli sayıda iletileri kadar her saniye gönderilir terval) gönderilmek üzere kullanılır. İletiler (hem istemci hem de sunucusundan) için belirli bir hızı kısıtlar örnek bir uygulama için bkz: [SignalR ile yüksek sıklıkta gerçek zamanlı](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>İleti boyutunu azaltma

Serileştirilmiş nesneler boyutunu azaltarak bir SignalR ileti boyutunu azaltabilirsiniz. Aktarılması gerekmez özellikleri içeren bir nesne gönderiyorsanız, sunucu kodunda bu özellikleri kullanarak serileştirilen gelen engelliyor `JsonIgnore` özniteliği. Özellik adlarını da iletide depolanır; Özellik adlarını kullanarak kısaltılmış `JsonProperty` özniteliği. Aşağıdaki kod örneği, istemciye gönderilen bir özelliği dışarıda tutması etme ve özellik adları kısaltın gösterir:

**İstemciye gönderilen veri dışlanacak JsonIgnore özniteliği ve ileti boyutunu azaltmak için Item özniteliğini gösterir .NET sunucu kodu**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Okunabilirliğini korumak amacıyla / Bakımı istemci kodu kısaltılmış özellik adları olabilir açmayla İnsan kolay için adları iletiyi aldıktan sonra. Aşağıdaki kod örneği, ileti sözleşmesi (eşleme) tanımlayarak uzun olanlara kısaltılmış adlarını yeniden eşleme, olası bir yol gösterir ve kullanarak `reMap` işlevi için en iyi duruma getirilmiş ileti sınıfı sözleşme uygulamak için:

**Yeniden harita oluşturma istemci tarafı JavaScript kodu okunabilir adlarına özellik adları kısaltılmış**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Adları aynı zamanda sunucu için istemci iletilerindeki yöntemin aynısı kullanılarak kısaltılması.

(Diğer bir deyişle, ileti için kullanılan bellek miktarı) bellek kullanım alanını azaltmaya ileti nesne da performansı geliştirebilirsiniz. Örneğin, tam aralığını bir `int` gerekli değildir, bir `short` veya `byte` yerine kullanılabilir.

Ayrıca, sunucu belleği, ileti boyutunu azaltma ileti veri yolunda iletileri depolandığından sunucu bellek sorunları gidermek erişebilirsiniz.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>SignalR sunucunuz için performans ayarlama

Aşağıdaki yapılandırma ayarları, bir SignalR uygulamasında daha iyi performans için sunucunuzu ayarlamak için kullanılabilir. Bir ASP.NET uygulamasında performansı hakkında genel bilgi için bkz: [ASP.NET performans geliştirme](https://msdn.microsoft.com/en-us/library/ff647787.aspx).

**SignalR yapılandırma ayarları**

- **DefaultMessageBufferSize**: varsayılan olarak, SignalR hub bağlantı başına başına bellek 1000 iletilerinde korur. Büyük iletileri kullanılıyorsa, bu da bu değeri azaltarak alleviated bellek sorunlar oluşturabilir. Bu ayar ayarlanabilir `Application_Start` olay işleyicisi bir ASP.NET uygulamasında veya içinde `Configuration` kendini barındıran bir uygulamada OWIN başlangıç sınıfı yöntemi. Aşağıdaki örnek, kullanılan sunucu bellek miktarını azaltmak için uygulamanızın bellek ayak izini azaltmak için bu değeri azaltmak gösterilmiştir:

    **Varsayılan ileti arabellek boyutunu azaltmak için haline .NET sunucu kodu**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**IIS yapılandırma ayarları**

- **Uygulama başına en fazla eşzamanlı istek**: eşzamanlı IIS sayısını artırmayı istekleri isteklere hizmet vermek için kullanılabilir sunucu kaynakları artacaktır. Varsayılan değer 5000'dir; Bu ayar artırmak için yükseltilmiş bir komut isteminde aşağıdaki komutları çalıştırın:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: Bu maksimum HTTP.sys'nin uygulama havuzu için sıraya koyar isteklerinin sayısıdır. Sıra dolduğunda yeni istekler 503 "Hizmet kullanılamıyor" yanıtı alırsınız. Varsayılan değer 1000'dir.

    Uygulamanızı barındıran uygulama havuzundaki çalışan işlem sırası uzunluğu kısaltmayı, bellek kaynaklarının tasarrufu. Daha fazla bilgi için bkz: [yönetme, ayarlama ve uygulama havuzlarını yapılandırma](https://technet.microsoft.com/en-us/library/cc745955.aspx).

**ASP.NET yapılandırma ayarları**

Bu bölümde ayarlanabilir yapılandırma ayarlarını içeren `aspnet.config` dosya. Bu dosya, platforma bağlı olarak iki konumlardan birinde bulunur:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

SignalR performansını artırabilir ASP.NET ayarları aşağıdakileri içerir:

- **CPU başına en fazla eşzamanlı istek**: Bu ayar artan performans sorunları hafifletmek. Bu ayar artırmak için şu yapılandırma ayarı ekleme `aspnet.config` dosyası:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **İstek sırası sınırı**: toplam bağlantı sayısını aştığında `maxConcurrentRequestsPerCPU` ayarlama, ASP.NET bir sıra kullanan isteklerinin azaltma başlar. Sıranın boyutunu artırmak için artırabilirsiniz `requestQueueLimit` ayarı. Bunu yapmak için şu yapılandırma ayarı ekleyin `processModel` düğümünde `config/machine.config` (yerine `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Performans sorunlarını giderme

Bu bölümde, uygulamanızda performans sorunları bulma yolları açıklanmaktadır.

### <a name="verifying-that-websocket-is-being-used"></a>WebSocket kullanılmakta olduğunu doğrulama

SignalR istemci ve sunucu arasında iletişim için aktarımları çeşitli kullanabilirsiniz, ancak WebSocket önemli performans avantajı sunar ve istemci ve sunucu destekliyorsa kullanılmalıdır. İstemci ve sunucu WebSocket gereksinimlerini karşılayıp karşılamadığını belirlemek için bkz: [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports). Hangi aktarım uygulamanızda kullanıldığını belirlemek için tarayıcının Geliştirici Araçları'nı kullanın ve hangi aktarım bağlantı için kullanıldığını görmek için günlüklerini inceleyin. Internet Explorer ve Chrome tarayıcı geliştirme araçları kullanma hakkında daha fazla bilgi için bkz: [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>SignalR performans sayaçlarını kullanma

Bu bölümde SignalR performans sayaçlarını, etkinleştirme ve nasıl kullanılacağını açıklar bulunan `Microsoft.AspNet.SignalR.Utils` paket.

### <a name="installing-signalrexe"></a>Signalr.exe yükleme

Performans sayaçları SignalR.exe adlı bir programı kullanarak sunucuya eklenebilir. Bu yardımcı programı yüklemek için aşağıdaki adımları izleyin:

1. Visual Studio uygulamanızı seçin **Araçları**, **kitaplık Paket Yöneticisi**, **çözüm için NuGet paketlerini Yönet...**
2. Arama **signalr.utils**ve yükleme seçeneğini belirleyin.

    ![](signalr-performance/_static/image1.png)
3. Paketi yüklemek için lisans sözleşmesini kabul edin.
4. SignalR.exe için yüklenecek `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Performans sayaçları ile SignalR.exe yükleniyor

SignalR performans sayaçlarını yüklemek için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

SignalR performans sayaçlarını kaldırmak için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>SignalR performans sayaçları

Yardımcı programlar paketi aşağıdaki performans sayaçlarını yükler. Son uygulama havuzu veya sunucu yeniden başlatıldığından beri "Toplam" sayaçları olayların sayısını ölçer.

**Bağlantı ölçümleri**

Aşağıdaki ölçümleri ortaya bağlantı ömür olayları ölçün. Daha fazla bilgi için bkz: [anlama ve bağlantı ömür olayları işleme](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Bağlı bağlantıları**
- **Bağlantısı yeniden kurulmuş bağlantıları**
- **Bağlantısı kesilmiş bağlantıları**
- **Bağlantılar-geçerli**

**İleti ölçümleri**

SignalR tarafından oluşturulan ileti trafiği ölçümleri.

- **Toplam alınan bağlantı iletisi**
- **Toplam gönderilen bağlantısı iletileri**
- **Bağlantı alınan ileti sayısı/sn**
- **Bağlantı gönderilen ileti sayısı/sn**

**İleti veri yolu ölçümleri**

İç SignalR ileti veri yolu, tüm gelen ve giden SignalR iletilerinin sıraya alınma sırası trafiğinin ölçümleri. Bir ileti **yayımlanan** zaman, gönderilen veya yayın. A **abone** bu bağlamda bir ileti veri yoluna abonelikte; bu istemciler ve sunucu sayısına eşit. Bir **ayrılmış çalışan** , etkin bağlantıları için; veri gönderen bir bileşen olan bir **meşgul çalışan** etkin olarak ileti gönderme biridir.

- **Alınan toplam ileti veri yoluna iletileri**
- **İleti veri yoluna iletileri alınan/sn**
- **Toplam ileti veri yoluna yayımlanan**
- **İleti veri yoluna iletileri yayımlanan/sn**
- **İleti veri yolu aboneleri geçerli**
- **İleti veri yolu aboneleri toplam**
- **İleti veri yolu aboneleri/sn**
- **Çalışanlar ayrılmış ileti veri yolu**
- **İleti veri yolu meşgul çalışanların**
- **İleti veri yolu konuları geçerli**

**Hata ölçümleri**

SignalR ileti trafiği tarafından oluşturulan hatalar ölçümleri. **Hub çözümleme** hataları hub veya hub yönteminin çözümlenemiyor olduğunda oluşur. **Hub çağrısının** hataları olan bir hub yöntemini çağırma sırasında oluşturulan özel durumları. **Aktarım** bir HTTP isteği veya yanıtı sırasında oluşturulan bağlantı hatalarını hatalardır.

- **Hatalar: Tüm toplam**
- **Hata: Tüm/sn**
- **Hata: Hub çözümleme toplam**
- **Hata: Hub çözümleme/sn**
- **Hatalar: Hub çağırma toplam**
- **Hata: Hub çağırma/sn**
- **Hata: Aktarım toplam**
- **Hata: Aktarım/sn**

<a id="scaleout_metrics"></a>

**Genişletme ölçümleri**

Aşağıdaki trafik ve genişletme sağlayıcı tarafından oluşturulan hatalar ölçümleri. A **akış** bu bağlamda bir ölçek birimi genişletme sağlayıcı tarafından kullanılan; SQL Server kullanılıyorsa bir tablo, Service Bus kullanılırsa, bir konu ve abonelik Redis kullandıysanız budur. Her akış sıralı okuma ve yazma işlemleri sağlar; tek bir akış olası bir ölçek performans sorunu olduğundan, bu performans sorunu azaltmaya yardımcı olmak için akış sayısı artırılabilir. Birden çok akış kullandıysanız, SignalR (parça) iletileri sırayla verilen tüm bağlantısından gönderilen iletileri garantiler şekilde bu akışları arasında otomatik olarak dağıtır.

[MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) SignalR tarafından tutulan genişletme gönderme sırası uzunluğu ayarı denetler. Bir değer için 0 birer birer için yapılandırılmış ileti devre kartı gönderilmek üzere bir gönderme sıradaki tüm iletileri yerleştirir daha büyük olarak ayarlama. Sıranın boyutunu yapılandırılan uzunluktan kalırsa, göndermek için sonraki çağrılar hemen başarısız bir [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) sırasındaki ileti sayısını ayarından küçük olana kadar yeniden. Uygulanan backplanes genellikle kendi queuing veya akış denetimi yerinde olduğundan çağrısı varsayılan olarak devre dışıdır. SQL Server söz konusu olduğunda, bağlantı havuzu herhangi bir zamanda geçmeden gönderir sayısını etkili bir şekilde sınırlar.

Varsayılan olarak, yalnızca bir akış SQL Server ve Redis için kullanılan, beş akışlar, hizmet veri yolu için kullanılır ve çağrısı devre dışıdır, ancak bu ayarları, SQL Server ve hizmet veri yolu üzerinde yapılandırma yoluyla değiştirilebilir:

**SQL Server devre kartı için tablo sayısı ve kuyruk uzunluğu yapılandırmak için .NET sunucu kodu**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**Hizmet veri yolu devre kartı için konu sayısı ve kuyruk uzunluğu yapılandırmak için .NET sunucu kodu**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

A **arabelleği** akış, hatalı bir duruma geçtiğini; akış hatalı durumda olduğunda hemen akış artık hatalı kadar devre kartına gönderilen tüm iletiler başarısız olur. **Gönderme sırası uzunluğu** gönderilen, ancak henüz gönderilen ileti sayısıdır.

- **Saniye başına alınan genişletme ileti veri yoluna iletileri**
- **Genişletme akışları toplam**
- **Açık genişletme akışları**
- **Arabelleğe alma genişletme akışlar**
- **Genişletme hataları toplam**
- **Genişletme Hatası/sn**
- **Genişletme gönderme sırası uzunluğu**

Bu sayaçlar ölçme daha fazla bilgi için bkz: [SignalR genişletme Azure Service Bus ile](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Diğer performans sayaçlarını kullanma

Aşağıdaki performans sayaçları da uygulamanızın performansını izleme yararlı olabilir.

**Bellek**

- .NET CLR bellek\\# bayt tüm yığınlardaki (w3wp)

**ASP.NET**

- ASP.NET\Requests geçerli
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- İşlemci Information\Processor zamanı

**TCP/IP'Yİ**

- TCPv6/kurulan bağlantılar
- TCPv4/kurulan bağlantılar

**Web hizmeti**

- Web Service\Current bağlantıları
- Web Service\Maximum bağlantıları

**İş parçacığı oluşturma**

- .NET CLR kilitler ve iş parçacığı\\geçerli mantıksal iş parçacığı sayısı
- .NET CLR kilitler ve iş parçacığı\\geçerli fiziksel iş parçacığı sayısı

<a id="otherresources"></a>

## <a name="other-resources"></a>Diğer Kaynaklar

ASP.NET Performans izleme ve ayarlama hakkında daha fazla bilgi için aşağıdaki konulara bakın:

- [ASP.NET Performans genel bakış](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)
- [IIS 7.5, IIS 7.0 ve IIS 6.0 ASP.NET iş parçacığı kullanımı](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; öğesi (Web Ayarları)](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
