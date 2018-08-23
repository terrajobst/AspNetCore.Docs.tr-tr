---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: SignalR izlemeyi etkinleştirme | Microsoft Docs
author: tfitzmac
description: Bu belge, etkinleştirmek ve SignalR sunucular ve istemciler için izlemeyi yapılandırmak açıklar. İzleme, olaylar hakkında tanılama bilgilerini görüntülemenize olanak tanır...
ms.author: riande
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: ee62a7b01ff357262aa89dbac4f49180b4c58fe0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753399"
---
<a name="enabling-signalr-tracing"></a>SignalR izlemeyi etkinleştirme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu belge, etkinleştirmek ve SignalR sunucular ve istemciler için izlemeyi yapılandırmak açıklar. İzleme, SignalR uygulamanızda olaylar hakkında tanılama bilgilerini görüntülemenize olanak tanır.
> 
> Bu konu başlığında, başlangıçta Patrick Fletcher tarafından alınmaktadır.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET Framework 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


İzleme etkin olduğunda, SignalR uygulama olayları için günlük girişleri oluşturur. Hem istemci hem de sunucu olayları oturum açabilirsiniz. Sunucu günlükleri bağlantısı, genişletme sağlayıcısındaki ve message bus olayları izleme. İstemci günlükleri bağlantı olayları izleme. SignalR 2.1 ve daha sonra istemci izleme tam içeriğini hub çağırma iletileri günlüğe kaydeder.

## <a name="contents"></a>İçindekiler

- [Sunucuda izlemeyi etkinleştirme](#server)

    - [Metin dosyaları için sunucu olayları günlüğe kaydetme](#server_text)
    - [Sunucu olayları için olay günlüğü](#server_eventlog)
- [.NET istemci (Windows Masaüstü uygulamaları) izlemeyi etkinleştirme](#net_client)

    - [Masaüstü İstemcisi olayları konsolda günlüğe kaydetme](#desktop_console)
    - [Bir metin dosyasına masaüstü istemcisi olayları günlüğe kaydetme](#desktop_text)
- [Windows Phone 8 istemcilerinde izlemeyi etkinleştirme](#phone)

    - [Windows Phone istemci olayları günlüğe kaydetme için kullanıcı Arabirimi](#phone_ui)
    - [Hata ayıklama konsoluna Windows Phone istemci olayları günlüğe kaydetme](#phone_debug)
- [JavaScript istemci izlemeyi etkinleştirme](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Sunucuda izlemeyi etkinleştirme

Uygulamanın yapılandırma dosyasına (App.config veya Web.config proje türüne bağlı olarak.) Sunucusu'nda izlemeyi etkinleştir Hangi olayların kategorilerini oturum istediğinizi belirtin. Yapılandırma dosyasında, ayrıca bir metin dosyası, Windows olay günlüğü veya uygulaması kullanarak özel bir günlük olaylarını günlüğe kaydedecek şekilde artırılmayacağını [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

İletileri aşağıdaki tür sunucusu olay kategorileri şunlardır:

| Kaynak | İletiler |
| --- | --- |
| SignalR.SqlMessageBus | SQL Message Bus genişletme sağlayıcı Kurulum, veritabanı işlemi, hata ve zaman aşımı olayları |
| SignalR.ServiceBusMessageBus | Hizmet veri yolu genişletme sağlayıcısını konu oluşturma ve aboneliği, hata ve mesajlaşma olayları |
| SignalR.RedisMessageBus | Redis genişletme sağlayıcı bağlantı, bağlantıyı kesme ve hata olayları |
| SignalR.ScaleoutMessageBus | İleti olayları ölçeğini genişletme |
| SignalR.Transports.WebSocketTransport | WebSocket aktarım bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları |
| SignalR.Transports.ServerSentEventsTransport | Bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları ServerSentEvents Aktarım |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame aktarım bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları |
| SignalR.Transports.LongPollingTransport | LongPolling aktarım bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları |
| SignalR.Transports.TransportHeartBeat | Bağlantı, bağlantıyı kesme ve keepalive olaylarını taşıma |
| SignalR.ReflectedHubDescriptorProvider | Hub'ı bulma olayları |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Metin dosyaları için sunucu olayları günlüğe kaydetme

Aşağıdaki kod, her olay kategorisi için izlemeyi etkinleştirme gösterir. Bu örnek uygulamanın metin dosyalarına olaylarını günlüğe kaydedecek şekilde yapılandırır.

**İzlemeyi etkinleştirmek için sunucu kodu XML**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

Yukarıdaki kodda `SignalRSwitch` girişi belirtir [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) belirtilen'ına gönderilen olaylar için kullanılır. Bu durumda, kümesine `Verbose` yani tüm hata ayıklama ve izleme iletilerini günlüğe kaydedilir.

Aşağıdaki çıktı girişlerinden gösterir `transports.log.txt` Yukarıdaki yapılandırma dosyası kullanarak bir uygulama için dosya. Yeni bir gösterir bağlantısı, bir bağlantı kaldırıldı ve aktarım sinyal olayları.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Sunucu olayları için olay günlüğü

Bir metin dosyası yerine olay günlüğüne olayları günlüğe kaydetmek için girişlere değerlerini değiştirmek `sharedListeners` düğümü. Aşağıdaki kod, sunucu olayları olay günlüğüne gösterilmektedir:

**Olayları olay günlüğüne XML sunucu kodu**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Olayların uygulama günlüğüne kaydedilir ve aşağıda gösterildiği gibi Olay Görüntüleyicisi ' kullanılabilir:

![SignalR günlükleri gösteren Olay Görüntüleyicisi](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Olay günlüğü kullanırken ayarlayın **TraceLevel** için **hata** ileti sayısı yönetilebilir tutmak için.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>.NET istemci (Windows Masaüstü uygulamaları) izlemeyi etkinleştirme

.NET istemci olayları konsola bir metin dosyası veya uygulaması kullanarak özel bir günlük için oturum [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

.NET istemci günlüğe kaydetmeyi etkinleştirmek için bağlantı ayarlayın. `TraceLevel` özelliğini bir [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) değeri ve `TraceWriter` geçerli bir özellik [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) örneği.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Masaüstü İstemcisi olayları konsolda günlüğe kaydetme

Aşağıdaki C# kodu, konsola .NET istemci olaylarını günlüğe kaydedecek şekilde gösterilmiştir:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Bir metin dosyasına masaüstü istemcisi olayları günlüğe kaydetme

Aşağıdaki C# kodu bir metin dosyasına .NET istemci olaylarını günlüğe kaydedecek şekilde gösterilmiştir:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

Aşağıdaki çıktı girişlerinden gösterir `ClientLog.txt` Yukarıdaki yapılandırma dosyası kullanarak bir uygulama için dosya. Sunucuya bağlanırken istemci gösterir ve bir istemci metodu çağrılırken hub'ı adlı `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Windows Phone 8 istemcilerinde izlemeyi etkinleştirme

SignalR uygulamalarını Windows Phone uygulamaları için aynı .NET istemci masaüstü uygulamaları kullanın ancak [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) ve bir dosyaya yazma [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) kullanılabilir değil. Bunun yerine, özel uygulanışı oluşturmak için ihtiyacınız [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) izleme. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Windows Phone istemci olayları günlüğe kaydetme için kullanıcı Arabirimi

[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) izleme çıktısına Yazar bir Windows Phone örneği içeren bir [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) özel kullanarak [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) adlı uygulama `TextBlockWriter`. Bu sınıf bulunabilir **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** proje. Bir örneğini oluştururken `TextBlockWriter`, geçerli geçirmek [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)ve [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) durum oluşturduğunuz bir [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) izleme için kullanılacak Çıkış:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

İzleme çıktısına sonra yeni bir yazılacak [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) oluşturulan [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) içinde geçirilen:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Hata ayıklama konsoluna Windows Phone istemci olayları günlüğe kaydetme

Kullanıcı Arabirimi yerine hata ayıklama konsoluna, çıkış göndermek için bir uygulamasını oluşturun [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) hata ayıklama penceresine yazar ve, bağlantının atama [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) özelliği:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

İzleme bilgileri daha sonra Visual Studio hata ayıklama penceresinde yazılır:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>JavaScript istemci izlemeyi etkinleştirme

Bir bağlantı üzerinde istemci tarafı günlüğe kaydetmeyi etkinleştirmek için ayarlanmış `logging` özelliği çağırmadan önce bağlantı nesnesindeki `start` bağlantı kurmak için yöntemi.

**İstemci JavaScript kodunu, tarayıcı konsoluna (ile oluşturulan proxy) izlemeyi etkinleştirme**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**İstemci JavaScript kodunu, tarayıcı konsoluna (olmadan oluşturulan proxy) izlemeyi etkinleştirme**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

İzleme etkin olduğunda, JavaScript istemcisi tarayıcı konsoluna olayları kaydeder. Tarayıcı konsoluna erişmek için bkz: [izleme taşımalar](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Aşağıdaki ekran görüntüsünde, SignalR JavaScript istemci izleme etkin gösterir. Bağlantı ve hub çağırma olayları tarayıcı konsolunda gösterir:

![SignalR tarayıcı konsolunda olayları izleme](enabling-signalr-tracing/_static/image4.png)
