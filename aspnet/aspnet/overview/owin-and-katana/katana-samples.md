---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana örnekleri | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: e81da1e650d8dfd24a3e0fda6aa42b7f360ce12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393325"
---
<a name="katana-samples"></a>Katana örnekleri
====================
tarafından [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana örnekleri

**Örnek ASP.NET yönlendiren** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
Bazı uygulamalarda, Asp.Net yol tablosundaki OWIN olmayan bileşenleri ile yan yana OWIN bileşenleri denetime isteyebilirsiniz. Bu örnek MapOwinPath ve Microsoft.Owin.Host.SystemWeb tarafından sağlanan MapOwinRoute RouteCollection uzantı yöntemlerinin nasıl kullanılacağını gösterir.

**İşlem hatları örnek dallanma** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
OWIN istek işleme ardışık düzenleri doğrusal olması gerekmez, bunlar farklı şekillerde isteklerini işlemek için dal. Bu örnek istek yollarını veya üst bilgileri gibi diğer istek verilerini temel alarak bir dallanma ardışık düzenini oluşturmak nasıl gösterir. Bu bileşenler Microsoft.Owin.Mapping nuget paketi kullanılabilir.

**Özel sunucu örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Kendi kendine barındırma, özel bir OWIN sunucusu kullanmayı gösterir OWIN.

**Örnek katıştırılmış** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Bazı OWIN sunucuları içinde kendi işlem çalıştırılabilir (&quot;şirket içinde barındırılan&quot;). Bu örnek Microsoft.Owin.Hosting nuget paketi tarafından sağlanan araçları kullanarak bir OWIN uygulamanın nasıl gösterir.

**HelloWorld örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN HTTP sunucusu, çeşitli sunucular arasında uygulama taşınabilirliği sağlayan API soyutlama ' dir. Bu örnek gösterir kullanarak bir Hello World uygulaması yazma **basit sarmalayıcıları** bir web sunucusu üzerinde ham OWIN soyutlama ve çalışma ASP.NET gibi.

**Merhaba Dünya ham OWIN örneği** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
Bu örnek, bir Hello World uygulaması kullanılarak yazılacak gösterilmiştir **ham** OWIN soyutlama ve Asp.Net gibi bir web sunucusunda.

**SignalR örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
OWIN kullanarak SignalR barındırma gösterilmektedir / Katana. Kendi kendine barındırma SignalR hakkında daha fazla bilgi için bkz. [öğretici: SignalR barındırma](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Statik dosyalar örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
OWIN kullanarak statik dosyaları için HTTP isteklerini destekleyecek şekilde nasıl gösterir / Katana.

**Web API** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Bu örnek, IIS'de OWIN barındırma ve Web API OWIN ardışık düzenine eklemek gösterilmektedir.

**Web yuvası örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Web yuvaları OWIN kullanarak desteklemek nasıl gösterir [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) sınıfı.
