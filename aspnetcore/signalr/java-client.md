---
title: ASP.NET Core SignalR Java istemci
author: mikaelm12
description: ASP.NET Core SignalR Java istemcisi kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482924"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR Java istemci

Tarafından [Mikael Mengistu](https://twitter.com/MikaelM_12)

Android uygulamaları dahil olmak üzere Java koddan bir ASP.NET Core SignalR sunucusuna bağlanan Java istemci sağlar. Gibi [JavaScript istemci](xref:signalr/javascript-client) ve [.NET istemci](xref:signalr/dotnet-client), almak ve gerçek zamanlı bir hub'ına ileti göndermek Java istemcisi sağlar. Java istemci, ASP.NET Core 2.2 bulunan ve üzerinde desteklenir.

Bu makalede bahsedilen örnek Java konsol uygulaması SignalR Java istemcisi kullanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>SignalR Java istemci paketini yükle

*0.1.0 preview2 35174 signalr* JAR dosyasını istemcilerin SignalR hub'ları bağlanmasına izin verir. JAR dosyası en son sürüm numarasını bulmak için bkz: [Maven arama sonuçları](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).

Gradle kullanıyorsanız, aşağıdaki satırı ekleyin `dependencies` bölümünü, *build.gradle* dosyası:

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

Maven kullanarak eklerseniz içinde aşağıdaki satırları `<dependencies>` öğesinin, *pom.xml* dosyası:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Bir hub'ına bağlama

Kurmak için bir `HubConnection`, `HubConnectionBuilder` kullanılmalıdır. Hub'ı URL'si ve günlük düzeyinde bir bağlantı oluşturulurken yapılandırılabilir. Gerekli tüm seçenekler herhangi birini çağırarak yapılandırma `HubConnectionBuilder` yöntemleri önce `build`. Bağlantıyı başlatmak `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

Bir çağrı `send` bir hub yöntemini çağırır. Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemleri çağırma

Kullanım `hubConnection.on` hub çağıran istemciye yöntemleri tanımlamak için. Yapı sonra ancak bağlantı başlatmadan önce yöntemleri tanımlar.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>Bilinen sınırlamalar

Bu bir Java istemci erken Önizleme sürümüdür. Henüz desteklenmeyen çok özellikleri vardır. Aşağıdaki boşlukları gelecek sürümleri için üzerinde çalışılan:

* Yalnızca ilkel türler, parametre olarak kabul edilebilir ve dönüş türleri.
* Zaman uyumlu apı'lerdir.
* Yalnızca "Gönder" çağrısı türü şu anda desteklenmiyor. "Çağır" ve dönüş değerleri akış desteklenmez.
* Yalnızca JSON Protokolü desteklenir.
* WebSockets taşıma desteklenmiyor.

## <a name="additional-resources"></a>Ek kaynaklar

* [Java API Başvurusu](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
