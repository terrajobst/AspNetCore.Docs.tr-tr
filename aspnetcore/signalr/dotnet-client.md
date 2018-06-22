---
title: ASP.NET Core SignalR .NET istemcisi
author: rachelappel
description: ASP.NET Core SignalR .NET istemcisi hakkında bilgi
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: faa4368988971a3e7fcdcd1b044971e16d70f19a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273301"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET istemcisi

Tarafından [Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR .NET istemci Xamarin, WPF, Windows Forms, konsol ve .NET Core uygulamalar tarafından kullanılabilir. Gibi [JavaScript istemci](xref:signalr/javascript-client), .NET istemci almak ve göndermek ve gerçek zamanlı olarak hub'ına iletileri almasına olanak tanır.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bu makaledeki kod örneği ASP.NET Core SignalR .NET istemcinin kullandığı bir WPF uygulamasıdır.

## <a name="install-the-signalr-net-client-package"></a>SignalR .NET istemci paketini yükle

`Microsoft.AspNetCore.SignalR.Client` Paket .NET istemcileri için SignalR hub'larını bağlamak gereklidir. İstemci Kitaplığı'nı yüklemek için aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresi:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Bir hub'a bağlanması

Bir bağlantı kurmak için oluşturma bir `HubConnectionBuilder` ve arama `Build`. Hub URL'si, protokolü, aktarım türü, günlük düzeyi, üstbilgiler ve diğer seçenekleri bağlantı oluşturulurken yapılandırılabilir. Gerekli tüm seçenekler herhangi birini ekleyerek yapılandırma `HubConnectionBuilder` yöntemlerin içine `Build`. Bağlantı ile başlangıç `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırın

`InvokeAsync` hub'ına yöntemleri çağırır. Hub yönteminin adını ve hub yönteminin tanımlanan herhangi bir bağımsız değişken geçirmek `InvokeAsync`. SignalR zaman uyumsuz ve bu nedenle kullanacağını `async` ve `await` çağrıları yapılırken.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemlerini çağırın

Hub'ı çağırır kullanarak yöntemlerini tanımlama `connection.On` yapı sonra ancak bağlantı başlatmadan önce.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

Önceki kod `connection.On` sunucu tarafı kodu kullanarak çağırdığında çalışır `SendAsync` yöntemi.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>Hata işleme ve günlüğe kaydetme

Try-catch deyimi hatalarla işleyin. İnceleme `Exception` hata oluştuktan sonra gerçekleştirilecek uygun eylemi belirlemek için nesne.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)