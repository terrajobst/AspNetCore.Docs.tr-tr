---
title: ASP.NET Core SignalR .NET istemcisi
author: bradygaster
description: ASP.NET Core SignalR .NET istemcisi hakkında bilgi
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: b59af0f9c84a008f778709970dba2273abdfcd4f
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087711"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET istemcisi

ASP.NET Core SignalR .NET istemci kitaplığı, .NET uygulamalarından SignalR hub'ları ile iletişim kurmanıza olanak sağlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Bu makalede kod örneği ASP.NET Core SignalR .NET istemcinin kullandığı bir WPF uygulamasıdır.

## <a name="install-the-signalr-net-client-package"></a>SignalR .NET istemci paketini yükle

`Microsoft.AspNetCore.SignalR.Client` Paket, .NET istemcileri için SignalR hub'larını bağlama için gereklidir. İstemci kitaplığını yüklemek için aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresi:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Bir hub'ına bağlama

Bir bağlantı kurmak için oluşturma bir `HubConnectionBuilder` ve çağrı `Build`. Hub'ı URL'si, protokolü, aktarım türü, günlük düzeyi, üst bilgiler ve diğer seçenekleri bir bağlantı oluşturulurken yapılandırılabilir. Gerekli tüm seçenekler ekleyerek herhangi bir yapılandırma `HubConnectionBuilder` yöntemleri `Build`. Bağlantıyı başlatmak `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Tanıtıcı bağlantısı kesildi

Kullanım <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> olay yanıt bağlantı kesildi. Örneğin, yeniden bağlanma otomatikleştirmek isteyebilirsiniz.

`Closed` Olay gerektirir döndüren bir temsilci bir `Task`, zaman uyumsuz kod kullanmadan çalıştırılmasına olanak sağlayan `async void`. Temsilci imzasında karşılamak için bir `Closed` çalıştırmaları eş döndüren bir olay işleyicisi `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

Zaman uyumsuz desteği için temel nedeni olduğundan, bağlantı yeniden başlatabilirsiniz. Bağlantı bir zaman uyumsuz eylem başlangıcıdır.

İçinde bir `Closed` bağlantı yeniden işleyicisi aşağıdaki örnekte gösterildiği gibi sunucu aşırı yüklemesini önlemek bazı rastgele gecikme bekleniyor dikkate alın:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

`InvokeAsync` hub yöntemleri çağırır. Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `InvokeAsync`. SignalR zaman uyumsuz, bu nedenle kullanın `async` ve `await` çağrıları yapılırken.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync` Yöntemi döndürür bir `Task` sunucu yöntem döndürüldüğünde tamamlar. Sonucu olarak dönüş değeri varsa, sağlanan `Task`. Sunucuda yöntemi tarafından oluşturulan özel durumlar bir hatalı üretmek `Task`. Kullanım `await` tamamlamak sunucu yöntemi için beklenecek söz dizimi ve `try...catch` sözdizimi hataları işlemek için.

`SendAsync` Yöntemi döndürür bir `Task` sunucuya ileti gönderildiğinde tamamlar. Dönüş değeri bu sağlanan `Task` Metoda tamamlanana kadar beklemez. İstemcide ileti gönderilirken karşılaşılan özel durumlar bir hatalı üretmek `Task`. Kullanım `await` ve `try...catch` işlemek için söz dizimi hatalarını gönderme.

> [!NOTE]
> Azure SignalR hizmeti kullanıyorsanız, *sunucusuz modu*, bir istemciden hub yöntemlerini çağıramazsınız. Daha fazla bilgi için [SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config).

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
* [Sunucusuz Azure SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config)
