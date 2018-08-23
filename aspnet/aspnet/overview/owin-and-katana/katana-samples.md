---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana örnekleri | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: b8ce2b40a19e0429f1ccedb03b8f829582652d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754452"
---
<a name="katana-samples"></a>Katana örnekleri
====================
tarafından [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana örnekleri

**Örnek ASP.NET yönlendiren** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
Bazı uygulamalarda, Asp.Net yol tablosundaki OWIN olmayan bileşenleri ile yan yana OWIN bileşenleri denetime isteyebilirsiniz. Bu örnek MapOwinPath ve Microsoft.Owin.Host.SystemWeb tarafından sağlanan MapOwinRoute RouteCollection uzantı yöntemlerinin nasıl kullanılacağını gösterir.

**İşlem hatları örnek dallanma** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
OWIN istek işleme ardışık düzenleri doğrusal olması gerekmez, bunlar farklı şekillerde isteklerini işlemek için dal. Bu örnek istek yollarını veya üst bilgileri gibi diğer istek verilerini temel alarak bir dallanma ardışık düzenini oluşturmak nasıl gösterir. Bu bileşenler Microsoft.Owin.Mapping nuget paketi kullanılabilir.

**Özel sunucu örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Kendi kendine barındırma, özel bir OWIN sunucusu kullanmayı gösterir OWIN.

**Örnek katıştırılmış** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)  
Bazı OWIN sunucuları içinde kendi işlem çalıştırılabilir (&quot;şirket içinde barındırılan&quot;). Bu örnek Microsoft.Owin.Hosting nuget paketi tarafından sağlanan araçları kullanarak bir OWIN uygulamanın nasıl gösterir.

**HelloWorld örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
OWIN HTTP sunucusu, çeşitli sunucular arasında uygulama taşınabilirliği sağlayan API soyutlama ' dir. Bu örnek gösterir kullanarak bir Hello World uygulaması yazma **basit sarmalayıcıları** bir web sunucusu üzerinde ham OWIN soyutlama ve çalışma ASP.NET gibi.

**Merhaba Dünya ham OWIN örneği** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)  
Bu örnek, bir Hello World uygulaması kullanılarak yazılacak gösterilmiştir **ham** OWIN soyutlama ve Asp.Net gibi bir web sunucusunda.

**SignalR örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)  
OWIN kullanarak SignalR barındırma gösterilmektedir / Katana. Kendi kendine barındırma SignalR hakkında daha fazla bilgi için bkz. [öğretici: SignalR barındırma](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Statik dosyalar örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
OWIN kullanarak statik dosyaları için HTTP isteklerini destekleyecek şekilde nasıl gösterir / Katana.

**Web API** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
Bu örnek, IIS'de OWIN barındırma ve Web API OWIN ardışık düzenine eklemek gösterilmektedir.

**Web yuvası örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
Web yuvaları OWIN kullanarak desteklemek nasıl gösterir [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) sınıfı.
