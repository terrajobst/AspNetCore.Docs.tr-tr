---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: SignalR izlemeyi etkinleştirme | Microsoft Docs
author: tfitzmac
description: Bu belge, etkinleştirmek ve SignalR sunucular ve istemciler için izlemeyi yapılandırmak açıklar. İzleme olayları hakkında tanılama bilgilerini görüntülemek sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: ac979acf162084a195bb769f842e77ad2498c7f3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28032824"
---
<a name="enabling-signalr-tracing"></a>SignalR izlemeyi etkinleştirme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu belge, etkinleştirmek ve SignalR sunucular ve istemciler için izlemeyi yapılandırmak açıklar. İzleme olayları hakkında tanılama bilgisi SignalR uygulamanızda görüntülemenizi sağlar.
> 
> Bu konuda, ilk olarak CAN Fletcher'dan tarafından yazıldı.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
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
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


İzleme etkinleştirildiğinde SignalR uygulama olayları için günlük girişleri oluşturur. Hem istemci hem de sunucu olayları oturum açabilir. Sunucu günlüklerini bağlantısı, genişletme sağlayıcısındaki ve message bus olayları izleme. İstemci günlükleri bağlantı olayları izleme. SignalR 2.1 ve daha sonra istemci izleme hub çağırma iletileri tam içeriğini günlüğe kaydeder.

## <a name="contents"></a>İçindekiler

- [Sunucunun izlemeyi etkinleştirme](#server)

    - [Metin dosyaları için sunucu olayları günlüğe kaydetme](#server_text)
    - [Sunucu olayları olay günlüğü](#server_eventlog)
- [.NET istemci (Windows Masaüstü uygulamaları) izlemeyi etkinleştirme](#net_client)

    - [Konsola masaüstü istemcisi olayları günlüğe kaydetme](#desktop_console)
    - [Bir metin dosyasına masaüstü istemcisi olayları günlüğe kaydetme](#desktop_text)
- [Windows Phone 8 istemcilerinde izlemeyi etkinleştirme](#phone)

    - [Windows Phone istemci olayları günlüğe kaydetmeyi için kullanıcı Arabirimi](#phone_ui)
    - [Hata ayıklama konsoluna Windows Phone istemci olayları günlüğe kaydetme](#phone_debug)
- [JavaScript istemci izlemeyi etkinleştirme](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Sunucunun izlemeyi etkinleştirme

Sunucu uygulamanın yapılandırma dosyasına (App.config veya Web.config proje türüne bağlı olarak.) içinde izlemeyi etkinleştir Kategorilerini olayların günlüğe kaydetmek istediğiniz belirtin. Yapılandırma dosyasında da mi bir metin dosyasına, Windows olay günlüğü veya uygulaması kullanarak özel bir günlük olaylarını günlüğe kaydedecek şekilde belirtmeniz [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Sunucu olay kategorileri iletilerinin aşağıdaki sıralar içerir:

| Kaynak | İletiler |
| --- | --- |
| SignalR.SqlMessageBus | SQL Message Bus genişletme sağlayıcı Kurulum, veritabanı işlemi, hata ve zaman aşımı olayları |
| SignalR.ServiceBusMessageBus | Hizmet veri yolu genişletme sağlayıcısı konu oluşturma ve aboneliği, hata ve mesajlaşma olayları |
| SignalR.RedisMessageBus | Redis genişletme sağlayıcı bağlantı, bağlantıyı kesme ve hata olayları |
| SignalR.ScaleoutMessageBus | Genişletme ileti olayları |
| SignalR.Transports.WebSocketTransport | WebSocket taşıma bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları |
| SignalR.Transports.ServerSentEventsTransport | Bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları ServerSentEvents taşıma |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame aktarım bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları |
| SignalR.Transports.LongPollingTransport | LongPolling aktarım bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları |
| SignalR.Transports.TransportHeartBeat | Bağlantı, bağlantıyı kesme ve keepalive olayları taşıma |
| SignalR.ReflectedHubDescriptorProvider | Hub bulma olayları |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Metin dosyaları için sunucu olayları günlüğe kaydetme

Aşağıdaki kod, her olay kategorisi izlemeyi etkinleştirmek gösterilmiştir. Bu örnek uygulama metin dosyalarına olaylarını günlüğe kaydedecek şekilde yapılandırır.

**İzlemeyi etkinleştirmek için XML sunucu kodu**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

Yukarıdaki kod `SignalRSwitch` girişi belirtir [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) belirtilen günlük gönderilen olaylar için kullanılır. Bu durumda, kümesine `Verbose` yani tüm hata ayıklama ve izleme iletilerini günlüğe kaydedilir.

Aşağıdaki çıkış girişlerinden gösterir `transports.log.txt` Yukarıdaki yapılandırma dosyası kullanarak bir uygulamanın dosyası. Yeni bir gösterir bağlantı, bağlantı kaldırıldı ve aktarım sinyal olaylar.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Sunucu olayları olay günlüğü

Bir metin dosyası yerine olay günlüğü olaylarını günlüğe kaydedecek şekilde girdileri değerlerini değiştirmek `sharedListeners` düğümü. Aşağıdaki kod, sunucu olayları olay günlüğüne gösterilmektedir:

**Olayları olay günlüğüne XML sunucu kodu**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Olayların uygulama günlüğüne kaydedilir ve aşağıda gösterildiği gibi Olay Görüntüleyicisi yoluyla kullanılabilir:

![SignalR günlüklerini gösteren Olay Görüntüleyicisi](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Olay günlüğü kullanırken ayarlayın **TraceLevel** için **hata** ileti sayısını yönetilebilir tutmak için.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>.NET istemci (Windows Masaüstü uygulamaları) izlemeyi etkinleştirme

.NET istemci olayları konsolu, bir metin dosyası veya uygulaması kullanarak özel bir günlük oturum [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

.NET istemci günlüğe kaydetmeyi etkinleştirmek için bağlantının ayarlayın `TraceLevel` özelliğine bir [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) değeri ve `TraceWriter` özelliğine geçerli bir [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) örneği.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Konsola masaüstü istemcisi olayları günlüğe kaydetme

Aşağıdaki C# kodu konsoluna .NET İstemcisi'nde olayları günlüğe gösterilmektedir:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Bir metin dosyasına masaüstü istemcisi olayları günlüğe kaydetme

Aşağıdaki C# kodu, bir metin dosyasına .NET İstemcisi'nde olayları günlüğe gösterilmektedir:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

Aşağıdaki çıkış girişlerinden gösterir `ClientLog.txt` Yukarıdaki yapılandırma dosyası kullanarak bir uygulamanın dosyası. Sunucuya bağlanırken istemci gösterir ve bir istemci yöntemi çağırma hub adı verilen `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Windows Phone 8 istemcilerinde izlemeyi etkinleştirme

SignalR uygulamalarını Windows Phone uygulamaları için aynı .NET istemcisi Masaüstü uygulamaları kullan ancak [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) ve bir dosyaya yazma [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) kullanılabilir değil. Bunun yerine, özel bir uygulamasını oluşturmak için gereken [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) izleme. 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Windows Phone istemci olayları günlüğe kaydetmeyi için kullanıcı Arabirimi

[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) İzleme çıktısı Yazar bir Windows Phone örneği içeren bir [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) özel kullanarak [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) adlı uygulama `TextBlockWriter`. Bu sınıf bulunabilir **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projesi. Bir örneğini oluştururken `TextBlockWriter`, geçerli geçirmek [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)ve bir [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) , burada oluşturacak bir [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) izlemesi kullanmak için Çıktı:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

İzleme çıkışının sonra yeni bir yazılır [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) oluşturulan [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) içinde geçirilen:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Hata ayıklama konsoluna Windows Phone istemci olayları günlüğe kaydetme

Kullanıcı Arabirimi yerine hata ayıklama konsoluna çıkış göndermek için uygulaması oluşturma [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) , hata ayıklama penceresine yazar ve, bağlantının atamak [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) özelliği:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

İzleme bilgilerini sonra Visual Studio hata ayıklama penceresinde yazılır:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>JavaScript istemci izlemeyi etkinleştirme

İstemci tarafı bir bağlantıda günlüğe kaydetmeyi etkinleştirmek üzere ayarlamak `logging` özelliği çağırmadan önce bağlantıyı nesne `start` bağlantı kurmak için yöntem.

**Tarayıcı konsoluna (ile oluşturulan proxy) izlemeyi etkinleştirmek için istemci JavaScript kodu**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Tarayıcı konsoluna (olmadan oluşturulan proxy) izlemeyi etkinleştirmek için istemci JavaScript kodu**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

İzleme etkinleştirildiğinde, JavaScript istemci tarayıcı konsoluna olayları kaydeder. Tarayıcı konsoluna erişmek için bkz: [izleme taşımaları](../getting-started/introduction-to-signalr.md#MonitoringTransports).

Aşağıdaki ekran görüntüsünde izlemenin etkin bir SignalR JavaScript istemci gösterir. Bağlantı ve hub çağırma olayları tarayıcı konsolunda gösterir:

![Tarayıcı konsoluna SignalR izleme olayları](enabling-signalr-tracing/_static/image4.png)
