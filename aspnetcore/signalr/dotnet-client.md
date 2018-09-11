---
title: ASP.NET Core SignalR .NET istemcisi
author: tdykstra
description: ASP.NET Core SignalR .NET istemcisi hakkında bilgi
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373325"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET istemcisi

Tarafından [Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR .NET istemci kitaplığı, .NET uygulamalarından SignalR hub'ları ile iletişim kurmanıza olanak sağlar.

> [!NOTE]
> Xamarin, Visual Studio sürümü için özel gereksinimleri vardır. Daha fazla bilgi için [SignalR istemci 2.1.1 Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bu makalede kod örneği ASP.NET Core SignalR .NET istemcinin kullandığı bir WPF uygulamasıdır.

## <a name="install-the-signalr-net-client-package"></a>SignalR .NET istemci paketini yükle

`Microsoft.AspNetCore.SignalR.Client` Paket, .NET istemcileri için SignalR hub'larını bağlama için gereklidir. İstemci kitaplığını yüklemek için aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresi:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Bir hub'ına bağlama

Bir bağlantı kurmak için oluşturma bir `HubConnectionBuilder` ve çağrı `Build`. Hub'ı URL'si, protokolü, aktarım türü, günlük düzeyi, üst bilgiler ve diğer seçenekleri bir bağlantı oluşturulurken yapılandırılabilir. Gerekli tüm seçenekler ekleyerek herhangi bir yapılandırma `HubConnectionBuilder` yöntemleri `Build`. Bağlantıyı başlatmak `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

`InvokeAsync` hub yöntemleri çağırır. Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `InvokeAsync`. SignalR zaman uyumsuz, bu nedenle kullanın `async` ve `await` çağrıları yapılırken.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemleri çağırma

Hub'ı kullanarak çağırdığı yöntemleri tanımlamak `connection.On` yapı sonra ancak bağlantı başlatmadan önce.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Önceki kodda `connection.On` sunucu tarafı kod kullanarak çağırdığında çalıştıran `SendAsync` yöntemi.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Hata işleme ve günlüğe kaydetme

Bir try-catch deyiminin hatalarla işleyin. İnceleme `Exception` nesnesine bir hata gerçekleştikten sonra gerçekleştirilecek uygun eylemi belirleyin.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
