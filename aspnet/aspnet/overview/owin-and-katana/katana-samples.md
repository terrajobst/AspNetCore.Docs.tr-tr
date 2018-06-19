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
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034496"
---
<a name="katana-samples"></a>Katana örnekleri
====================
tarafından [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana örnekleri

**ASP.NET yönlendirir örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
Bazı uygulamalarda OWIN olmayan bileşenleri yan yana Asp.Net rota tablosunda OWIN bileşenleri bağlanacağını isteyeceksiniz. Bu örnek MapOwinPath ve Microsoft.Owin.Host.SystemWeb tarafından sağlanan MapOwinRoute RouteCollection genişletme yöntemleri kullanmayı gösterir.

**Ardışık düzenleri örnek dallanma** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
OWIN istek işleme ardışık düzenleri doğrusal olması gerekmez, bunlar farklı şekillerde isteklerini işlemek için dal. Bu örnek istek yollarını ya da diğer istek verileri üstbilgileri gibi göre dallanma ardışık düzenini oluşturmak nasıl gösterir. Bu bileşenler Microsoft.Owin.Mapping nuget paketi kullanılabilir.

**Özel sunucu örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
Kendi kendini barındırdığında özel bir OWIN sunucusu kullanmayı gösterir OWIN.

**Örnek katıştırılmış** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
Bazı OWIN sunucuları kendi işlem içinde çalıştırabilirsiniz (&quot;kendi kendini barındıran&quot;). Bu örnek Microsoft.Owin.Hosting nuget paketi tarafından sağlanan araçları kullanarak OWIN uygulamasının başlangıç gösterilmektedir.

**HelloWorld örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN HTTP sunucusu çeşitli sunucular arasında uygulama taşınabilirliği sağlayan API soyutlama ' dir. Bu örneği kullanarak bir Hello World uygulamasının nasıl yazılacağını gösterir **basit sarmalayıcıları** ham OWIN soyutlama ve Çalıştır bir web sunucusunda ister ASP.NET.

**Hello World ham OWIN örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
Bu örnek uygulama kullanarak bir Hello World yazmak gösterilmiştir **ham** OWIN soyutlama ve Asp.Net gibi bir web sunucusunda.

**SignalR örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
OWIN kullanarak SignalR kendini barındırma gösterilmektedir / Katana. Kendi kendine barındırma SignalR hakkında daha fazla bilgi için bkz: [Öğreticisi: SignalR kendini barındırma](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Statik dosyaları örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
HTTP isteklerini OWIN kullanarak statik dosyaları için destek gösterilmektedir / Katana.

**Web API** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
Bu örnek, IIS'de OWIN barındırma ve Web API OWIN ardışık düzenine eklemek gösterilmektedir.

**Web yuvası örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
Web yuvalarını kullanarak OWIN içinde desteklemek nasıl gösterir [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) sınıfı.
